
---

# 🚀 1. Что такое NestJS

**NestJS** — это backend-фреймворк для Node.js, построенный поверх Express (или Fastify), с архитектурой как в Angular.

👉 Главная идея:
**всё разбито на модули, сервисы и контроллеры**

---

# 🧱 2. Основные части

## 📦 Module (модуль)

Группирует логику

```ts
@Module({
  controllers: [UserController],
  providers: [UserService],
})
export class UserModule {}
```

👉 Это как "папка" с логикой

---

## 🎮 Controller (контроллер)

Обрабатывает HTTP-запросы

```ts
@Controller('users')
export class UserController {
  @Get()
  getAll() {
    return 'All users';
  }
}
```

👉 Отвечает за **роуты**

---

## ⚙️ Service (сервис)

Бизнес-логика

```ts
@Injectable()
export class UserService {
  getUsers() {
    return ['John', 'Mike'];
  }
}
```

👉 Контроллер → вызывает сервис

---

# 🔁 3. Dependency Injection (очень важно)

Nest сам создаёт зависимости

```ts
constructor(private userService: UserService) {}
```

👉 Ты **не создаёшь объекты вручную**

---

# 🌐 4. Основные декораторы

### Роуты:

```ts
@Get()
@Post()
@Put()
@Delete()
```

### Параметры:

```ts
@Get(':id')
get(@Param('id') id: string) {}
```

```ts
@Post()
create(@Body() dto: CreateUserDto) {}
```

---

# 🧾 5. DTO (Data Transfer Object)

Описывает структуру данных

```ts
export class CreateUserDto {
  name: string;
  age: number;
}
```

👉 Часто используется с библиотекой:

* class-validator

---

# ✅ 6. Валидация

```ts
export class CreateUserDto {
  @IsString()
  name: string;
}
```

Включение глобально:

```ts
app.useGlobalPipes(new ValidationPipe());
```

---

# 🔐 7. Middleware / Guards / Pipes

## 🧩 Middleware

Запускается ДО запроса

👉 логирование, auth

---

## 🛡 Guards

Решают: **пустить или нет**

```ts
@UseGuards(AuthGuard)
```

---

## 🔄 Pipes

Трансформируют/валидируют данные

---

# 🗄 8. Работа с БД

Чаще всего используют:

* TypeORM
* Prisma

Пример с Prisma:

```ts
@Injectable()
export class UserService {
  constructor(private prisma: PrismaService) {}

  findAll() {
    return this.prisma.user.findMany();
  }
}
```

---

# 📡 9. Асинхронность

Почти всё — async

```ts
@Get()
async getUsers() {
  return await this.userService.getUsers();
}
```

---

# 🧪 10. CLI (очень ускоряет работу)

Nest CLI:

```bash
npm i -g @nestjs/cli
nest new project
```

Создание:

```bash
nest g module users
nest g controller users
nest g service users
```

---

# 🧠 11. Архитектура (золотое правило)

👉 Всегда разделяй:

* Controller → принимает запрос
* Service → логика
* Module → связывает всё

---

# ⚡ 12. Минимальный поток запроса

```
Request → Controller → Service → DB → Response
```

---

# 🧩 13. Интерцепторы (продвинутый уровень)

Перехватывают ответ

👉 логирование, форматирование

---

# 📌 14. Частые ошибки новичков

❌ Смешивают логику в контроллере
❌ Не используют DTO
❌ Игнорируют DI
❌ Делают всё в одном файле

---

# 🧭 15. Как быстро начать

1. Установи Nest CLI
2. Создай проект
3. Сделай CRUD (users)
4. Подключи БД (Prisma)
5. Добавь валидацию
6. Сделай auth (JWT)

---

# 💡 Итог

NestJS — это про:

* чистую архитектуру
* масштабируемость
* понятную структуру

---
