# Проект: JS -> TS

## Цель

Переписать существующее приложение из [mobdev-lab11](https://github.com/41ISP/mobdev-lab11) на TypeScript

## Алгоритм

- Форкаете прошлую работу (если не проверил, то уберите галочку при форку, чтобы все ветки форкнулись)
- Сливаете wip/dev в main
- Инициализируете в корне Vite проект (React/TypeScript) (задайте название `название_проекта_который_был-ts`)
- Переносите из JavaScript проекта в TypeScript (не забудьте, что файлы у вас теперь будут называться ts и tsx)

## Типизация React-компонентов

### Функциональные компоненты с пропсами

```typescript
// Создаём интерфейс для пропсов
interface UserCardProps {
  name: string;
  age: number;
  isActive?: boolean; // опциональное поле
}

// Используем в компоненте
const UserCard = ({ name, age, isActive = true }: UserCardProps) => {
  return <div>{name}, {age}</div>;
};
```

### Типизация props.children

```typescript
interface LayoutProps {
  children: React.ReactNode;
  title: string;
}

const Layout = ({ children, title }: LayoutProps) => {
  return (
    <div>
      <h1>{title}</h1>
      {children}
    </div>
  );
};
```

## 🔄 Типизация данных API

### Интерфейсы для ответов API

```typescript
// Описываем структуру данных, которые приходят с сервера
interface User {
    id: number
    email: string
    firstName: string
    lastName: string
}

interface ApiResponse<T> {
    data: T
    status: string
    message?: string
}
```

### Типизация fetch-запросов

```typescript
const fetchUsers = async (): Promise<User[]> => {
    const response = await fetch('/api/users')
    const data: User[] = await response.json()
    return data
}

// С обработкой обёртки API
const getUser = async (id: number): Promise<ApiResponse<User>> => {
    const response = await fetch(`/api/users/${id}`)
    const data: ApiResponse<User> = await response.json()
    return data
}
```

## Типизация хуков

### useState

```typescript
// TypeScript часто выводит тип автоматически
const [count, setCount] = useState(0) // number
const [name, setName] = useState('') // string

// Явное указание типа (для объектов и массивов)
const [user, setUser] = useState<User | null>(null)
const [users, setUsers] = useState<User[]>([])
```

### Типизация асинхронных функций в useEffect

```typescript
useEffect(() => {
    const loadData = async () => {
        const data: User[] = await fetchUsers()
        setUsers(data)
    }

    loadData()
}, [])
```

## 🎯 События

### Обработчики событий

```typescript
// События формы
const handleSubmit = (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault()
}

// События инпутов
const handleChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    setValue(event.target.value)
}

// Клик по кнопке
const handleClick = (event: React.MouseEvent<HTMLButtonElement>) => {
    console.log('Clicked')
}
```

## 🧭 React Router

### Типизация навигации и параметров

```typescript
import { useParams, useNavigate } from 'react-router-dom';

// Типизация параметров маршрута
interface RouteParams {
  id: string;
}

const UserPage = () => {
  const { id } = useParams<RouteParams>();
  const navigate = useNavigate();

  const goBack = () => navigate('/users');

  return <div>User ID: {id}</div>;
};
```

## Документация

- [React]()
- [TypeScript]()
- [React Router]()
- []()

# Как сдавать

- Создайте форк репозитория в вашей организации с названием-этого-репозитория-вашафамилия
- Используя ветку wip сделайте задание
- Зафиксируйте изменения в вашем репозитории
- Когда документ будет готов - создайте пул реквест из ветки wip (вашей) на ветку main (тоже вашу) и укажите меня (ktkv419) как reviewer

Не мержите сами коммит, это сделаю я после проверки задания
