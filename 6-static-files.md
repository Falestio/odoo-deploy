## Default access static file
semua file statis di folder static bisa langsung di akses dari path ke modulnya
`/MODULE/static/FILE`

TODO: pahami tentang content security policy 
https://www.odoo.com/documentation/18.0/administration/on_premise/deploy.html#serving-static-files

## Odoo Attachment
- file yang disimpan di database odoo
- filestore yang di regulate odoo
- bisa setup static web server khusus
    - X-Sendfile (apache) 
    - X-Accel (nginx)

