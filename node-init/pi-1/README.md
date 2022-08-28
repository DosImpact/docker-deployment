# PI - 1 : Production Stack

- Application 서비스를 제공합니다.

```
Welcome Algoridang Stage

PI-1 React + NestJS + Redis 담당
PI-2 DB 서버를 담당
```

## 80
- algoridang_react
- sudo npx serve -l tcp://0.0.0.0:80

## 4000
- algoridang_nestjs

## 5000
- algoridang_ds

## 6000
- redis_algoridang_dev

## 7000
- redis_algoridang_q_dev


## 8000
- S3Clone StaticServing
- sudo pm2 start ecosystem.config.cjs --env production
