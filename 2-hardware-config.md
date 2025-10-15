## Server builtin odoo

- HTTP
- cron
- live chat

### Multi thread (default)
- simple server untuk development
- new thread spawned for every new HTTP Request
    - deamon cron thread summoned too
- aktif jika worker tidak diisi atau 0
- uses python GIL

### Multi processing
- worker created in server tartup
- HTTPS request queued > assigned to worker
- + worker for livechat in alternative port
- + worker for cron
- a process monitor worker and can kill/restart failed worker
- aktif kan dengan set worker

#### Worker calculation
Jumlah worker = (jumlah CPU * 2) + 1  

- Cron workers need CPU
- 1 worker ≈ 6 concurrent users

**Contoh:**  
Jika Anda memiliki 4 core CPU:  
Jumlah worker = (4 * 2) + 1 = **9 worker**

#### Memory Calculation
Kita anggap 20% request adalah heavy request, 80% adalah light request.

- Heavy worker (request berat, SQL/compute optimal): estimasi konsumsi RAM sekitar **1GB**
- Light worker (request ringan): estimasi konsumsi RAM sekitar **150MB**

Rumus:  
Needed RAM = #worker × ( (light_worker_ratio × light_worker_ram_estimation) + (heavy_worker_ratio × heavy_worker_ram_estimation) )

- light_worker_ratio = 0.8
- heavy_worker_ratio = 0.2
- light_worker_ram_estimation = 150MB
- heavy_worker_ram_estimation = 1GB

**Contoh perhitungan RAM untuk 9 worker:**  
Needed RAM = 9 × ( (0.8 × 150MB) + (0.2 × 1GB) )  
= 9 × (120MB + 200MB)  
= 9 × 320MB  
= **2880MB** (≈ 2.8GB)

Jadi, untuk 4 core CPU (9 worker), RAM yang dibutuhkan sekitar **2.8GB**.

---

**Jika heavy worker adalah 50%:**

- light_worker_ratio = 0.5
- heavy_worker_ratio = 0.5

Needed RAM = 9 × ( (0.5 × 150MB) + (0.5 × 1GB) )  
= 9 × (75MB + 500MB)  
= 9 × 575MB  
= **5175MB** (≈ 5.2GB)

Jadi, jika heavy worker 50%, RAM yang dibutuhkan sekitar **5.2GB** untuk 9 worker.

#### Example case

**Server with 4 CPU, 8 Thread**

##### 50 concurrent users

- 50 users / 6 = ~8.3 → gunakan **8 worker**
- (4 * 2) + 1 = **9** ← maksimal worker
- Gunakan 8 worker + 1 untuk cron
- **CPU optimal:** ceil((8 - 1) / 2) = ceil(7 / 2) = **4 CPU**
- RAM = 9 × ((0.8 × 150) + (0.2 × 1024)) ≈ 9 × (120 + 204.8) ≈ 9 × 324.8 ≈ **2.9GB RAM** untuk Odoo

##### 100 concurrent users

- 100 users / 6 = ~16.7 → gunakan **16 worker**
- (4 * 2) + 1 = **9** ← maksimal worker (jika ingin lebih, perlu upgrade CPU)
- **CPU optimal:** ceil((16 - 1) / 2) = ceil(15 / 2) = **8 CPU**
- RAM = 9 × ((0.8 × 150) + (0.2 × 1024)) ≈ **2.9GB RAM** (jika tetap pakai 9 worker)
- RAM = 16 × ((0.8 × 150) + (0.2 × 1024)) ≈ 16 × 324.8 ≈ **5.2GB RAM** (jika pakai 16 worker)

##### 150 concurrent users

- 150 users / 6 = 25 → gunakan **25 worker**
- (4 * 2) + 1 = **9** ← maksimal worker (jika ingin lebih, perlu upgrade CPU)
- **CPU optimal:** ceil((25 - 1) / 2) = ceil(24 / 2) = **12 CPU**
- RAM = 9 × ((0.8 × 150) + (0.2 × 1024)) ≈ **2.9GB RAM** (jika tetap pakai 9 worker)
- RAM = 25 × ((0.8 × 150) + (0.2 × 1024)) ≈ 25 × 324.8 ≈ **8.1GB RAM** (jika pakai 25 worker)

##### 200 concurrent users

- 200 users / 6 = ~33.3 → gunakan **33 worker**
- (4 * 2) + 1 = **9** ← maksimal worker (jika ingin lebih, perlu upgrade CPU)
- **CPU optimal:** ceil((33 - 1) / 2) = ceil(32 / 2) = **16 CPU**
- RAM = 9 × ((0.8 × 150) + (0.2 × 1024)) ≈ **2.9GB RAM** (jika tetap pakai 9 worker)
- RAM = 33 × ((0.8 × 150) + (0.2 × 1024)) ≈ 33 × 324.8 ≈ **10.7GB RAM** (jika pakai 33 worker)

```
[options]
limit_memory_hard = 1677721600
limit_memory_soft = 629145600
limit_request = 8192
limit_time_cpu = 600
limit_time_real = 1200
max_cron_threads = 1
workers = 8
```

### Linux native
Multi processing is LINUX ONLY

