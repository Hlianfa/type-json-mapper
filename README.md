# type-json-mapper

## Introduction

Instead of directly using api data, we definitely require an adapter layer to transform data as needed. Furthermore, the adapter inverse the the data dependency from API server(API Server is considered uncontrollable and highly unreliable as data structure may be edit by backend coder for some specific purposes)to our adapter which becomes reliable. Thus, this library is created as the adapter.

### Get Started

```bash
npm install type-json-mapper --save
```

## Language

- Typescript

## Example

Here is a complex example, hopefully could give you an idea of how to use it:

```typescript
import { mapperProperty } from "type-json-mapper";

class Lesson {
  @mapperProperty("ClassName")
  public name: string;
  @mapperProperty("Teacher")
  public teacher: string;
  @mapperProperty("DateTime", "datetime")
  public datetime: string;

  constructor() {
    this.name = "";
    this.teacher = "";
    this.datetime = "";
  }
}
class Address {
  public province: string;
  public city: string;
  @mapperProperty("full_address")
  public fullAddress: string;

  constructor() {
    this.province = "";
    this.city = "";
    this.fullAddress = "";
  }
}

class Student {
  @mapperProperty("StudentID", "string")
  public id: string;
  @mapperProperty("StudentName", "string")
  public name: string;
  @mapperProperty("StudentAge", "int")
  public age: number;
  @mapperProperty("StudentSex", "int")
  public sex: number;
  @deepMapperProperty("Address", Address)
  public address?: Address;
  @deepMapperProperty("Lessons", Lesson)
  public lessons?: Lesson[];

  constructor() {
    this.id = "";
    this.name = "";
    this.age = 0;
    this.sex = 0;
    this.address = undefined;
    this.lessons = undefined;
  }
}
```

Now here is what API server return, assume it is already parsed to JSON object.

```typescript
let json = {
  StudentID: "123456",
  StudentName: "李子明",
  StudentAge: "9",
  StudentSex: "1",
  Address: {
    province: "广东",
    city: "深圳",
    full_address: "xxx小学三年二班",
  },
  Lessons: [
    {
      ClassName: "中国上下五千年",
      Teacher: "建国老师",
      DateTime: 1609430399000,
    },
  ],
};
```

Simply, just map it use following code. The mapping is based on <@JsonProperty> decorator meta data.

```typescript
import { deserialize } from 'type-json-mapper';

const student = deserialize(Student, json);

```
[result_file]:data:image/png;base64,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

![result image][result_file]