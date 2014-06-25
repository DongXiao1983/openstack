1. /etc/keystone/keystone.conf

admin_token=ADMIN
admin_bind_host=100.0.0.11
admin_port=35357
public_port=5000

[auth]
methods=external,password,token
password=keystone.auth.plugins.password.Password
token=keystone.auth.plugins.token.Token

[catalog]
driver=keystone.catalog.backends.sql.Catalog

[database]
connection = mysql://keystone:keystone@100.0.0.11/keystone

[identity]
driver=keystone.identity.backends.sql.Identity

2. openstack-config --set /etc/keystone/keystone.conf database connection mysql://keystone:keystone@100.0.0.11/keystone    

3. Create DB

4. Create user

5. Add permission

5. Sync DB

6. export TOKEN & ENDPOINT     

7. Create roles

8. Create tenata

9. Add role

10. Create endpoint
