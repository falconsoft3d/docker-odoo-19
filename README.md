# docker-odoo-19

# Clonar el repo
```

cd docker-odoo-19
```


# Levantar el docker
```
docker-compose up -d
```

# Listar los dockers
```
docker ps
```

# error comun
```
docker-compose exec -u 0 odoo bash -lc "mkdir -p /var/lib/odoo/sessions && chown -R odoo:odoo /var/lib/odoo && chmod -R 700 /var/lib/odoo/sessions"
docker-compose restart odoo
```

# reiniciar el docker 
```
docker-compose restart odoo
docker-compose logs -f odoo
```

# ver los logs
```
docker-compose logs --tail=200 odoo
docker logs --tail=30 odoo19
docker-compose logs -f db
```
