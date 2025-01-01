# Advanced Types ใน TypeScript

## 1. Generics
Generics ใน TypeScript ช่วยให้เราสามารถเขียนโค้ดที่ยืดหยุ่นและนำกลับมาใช้ใหม่ได้ โดยรองรับหลายชนิดข้อมูล

### ทฤษฎีและการใช้งาน
```typescript
// ฟังก์ชัน Generic พื้นฐาน
function identity<T>(arg: T): T {
    return arg;
}

// การใช้งาน
let result1 = identity<string>("Hello");  // ระบุ type ชัดเจน
let result2 = identity(42);               // type inference

// Generic Interface
interface Container<T> {
    value: T;
    getValue(): T;
}

// Generic Class
class Box<T> {
    private content: T;

    constructor(value: T) {
        this.content = value;
    }

    getValue(): T {
        return this.content;
    }
}

// ตัวอย่างการใช้งานจริง
let numberBox = new Box<number>(123);
let stringBox = new Box<string>("Hello");
```

## 2. Utility Types
TypeScript มี built-in utility types ที่ช่วยในการจัดการ type ต่างๆ

### ตัวอย่าง Utility Types ที่ใช้บ่อย
```typescript
// Partial<T> - ทำให้ทุก property เป็น optional
interface User {
    id: number;
    name: string;
    email: string;
}

type PartialUser = Partial<User>;  // ทุก field เป็น optional

// Pick<T, K> - เลือกเฉพาะ property ที่ต้องการ
type UserBasicInfo = Pick<User, 'name' | 'email'>;

// Omit<T, K> - ตัด property ที่ไม่ต้องการออก
type UserWithoutId = Omit<User, 'id'>;

// Record<K, T> - สร้าง type ที่มี key และ value ตามที่กำหนด
type UserRoles = Record<string, string>;

// Required<T> - ทำให้ทุก property เป็น required
type RequiredUser = Required<PartialUser>;
```

## 3. Type Assertions
การระบุ type ที่แน่นอนเมื่อ TypeScript ไม่สามารถเดา type ได้อย่างถูกต้อง

```typescript
// แบบใช้ as
let someValue: unknown = "Hello TypeScript";
let strLength: number = (someValue as string).length;

// แบบใช้ angle-bracket (ไม่แนะนำใน JSX)
let otherValue: unknown = "Hello World";
let otherLength: number = (<string>otherValue).length;

// ตัวอย่างการใช้งานจริง
interface ApiResponse {
    data: unknown;
}

function processResponse(response: ApiResponse) {
    if (typeof response.data === 'string') {
        // Type assertion เพื่อจัดการกับ data ที่เป็น string
        const strData = response.data as string;
        return strData.toUpperCase();
    }
}
```

## 4. Intersection Types
การรวม type หลายๆ type เข้าด้วยกันโดยใช้เครื่องหมาย &

```typescript
// การสร้าง intersection type
interface HasName {
    name: string;
}

interface HasAge {
    age: number;
}

type Person = HasName & HasAge;

// การใช้งาน
const person: Person = {
    name: "สมชาย",
    age: 30
};

// ตัวอย่างการใช้งานจริง
interface ErrorHandling {
    success: boolean;
    error?: { message: string };
}

interface ApiData {
    data: unknown;
    timestamp: number;
}

type ApiResponse = ErrorHandling & ApiData;
```

## 5. Type Guards และ Narrowing
การตรวจสอบ type ในขณะ runtime เพื่อจำกัดขอบเขตของ type

```typescript
// Type guard function
function isString(value: unknown): value is string {
    return typeof value === 'string';
}

// การใช้ type guard
function processValue(value: string | number) {
    if (typeof value === 'string') {
        // ในบล็อกนี้ TypeScript รู้ว่า value เป็น string
        console.log(value.toUpperCase());
    } else {
        // ในบล็อกนี้ TypeScript รู้ว่า value เป็น number
        console.log(value.toFixed(2));
    }
}

// Instance type checking
class Animal {
    name: string;
    constructor(name: string) {
        this.name = name;
    }
}

class Dog extends Animal {
    bark(): void {
        console.log('Woof!');
    }
}

function isDog(animal: Animal): animal is Dog {
    return animal instanceof Dog;
}
```

## 6. Unknown และ Never Types

### Unknown Type
```typescript
// Unknown เป็น type-safe version ของ any
let userInput: unknown;
let userName: string;

userInput = "Hello";
// userName = userInput; // Error: Type 'unknown' is not assignable to type 'string'

// ต้องตรวจสอบ type ก่อนใช้งาน
if (typeof userInput === 'string') {
    userName = userInput; // OK
}
```

### Never Type
```typescript
// ฟังก์ชันที่ไม่มีวันทำงานเสร็จ
function throwError(message: string): never {
    throw new Error(message);
}

// ฟังก์ชันที่มี infinite loop
function infiniteLoop(): never {
    while (true) {}
}
```

## ตัวอย่างการใช้งาน Advanced Types ในสถานการณ์จริง

```typescript
// ระบบจัดการ State ของแอพพลิเคชัน
interface State<T> {
    data: T;
    loading: boolean;
    error: string | null;
}

// Utility function สำหรับสร้าง initial state
function createInitialState<T>(): State<T> {
    return {
        data: null as unknown as T,
        loading: false,
        error: null
    };
}

// Type guard สำหรับตรวจสอบ error response
interface ErrorResponse {
    error: string;
    code: number;
}

function isErrorResponse(value: unknown): value is ErrorResponse {
    return (
        typeof value === 'object' &&
        value !== null &&
        'error' in value &&
        'code' in value
    );
}

// การใช้งานทั้งหมดรวมกัน
async function fetchData<T>(url: string): Promise<State<T>> {
    const state = createInitialState<T>();

    try {
        const response = await fetch(url);
        const data = await response.json();

        if (isErrorResponse(data)) {
            return {
                ...state,
                error: data.error
            };
        }

        return {
            data: data as T,
            loading: false,
            error: null
        };
    } catch (error) {
        return {
            ...state,
            error: error instanceof Error ? error.message : 'Unknown error'
        };
    }
}
```

## แบบฝึกหัด

1. สร้าง Generic function สำหรับ filter array
2. ใช้ Utility Types ในการสร้าง form validation
3. สร้าง Type Guard สำหรับตรวจสอบ API Response
4. พัฒนาระบบ State Management โดยใช้ Advanced Types

