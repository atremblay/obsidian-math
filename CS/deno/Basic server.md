
A basic class for an Employee

```typescript
// file employee.ts

export class Employee{
    name: string;
    id: number;
    position: string;

    constructor(_id: number, _name: string, _position: string){
        this.id = _id;
        this.name = _name;
        this.position = _position;
    }
};
```

Two ways of creating a server. Either with the object Server

```typescript
import { Server } from "https://deno.land/std@0.180.0/http/server.ts";
import { Employee } from './employee.ts';

const employees: Employee[] = [];
employees.push(new Employee(1, 'Sam', 'Manager'));
employees.push(new Employee(2, 'Michael', 'Worker'));
employees.push(new Employee(3, 'Antonio', 'Accountant'));

const port = 4505;

const handler = (request: Request) => {
    return new Response(JSON.stringify(employees), {status:200});
};

const server = new Server({ port, handler });

await server.listenAndServe();
```


Or with the serve function. As of now, no idea why we should use one or the other.
```typescript
import { serve } from "https://deno.land/std@0.180.0/http/server.ts";
import { Employee } from './employee.ts';

const employees: Employee[] = [];
employees.push(new Employee(1, 'Sam', 'Manager'));
employees.push(new Employee(2, 'Michael', 'Worker'));
employees.push(new Employee(3, 'Antonio', 'Accountant'));

const port = 4505;

serve((_req) => new Response(JSON.stringify(employees), {status:200}), {port:port});
```

