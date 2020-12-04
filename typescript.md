## Typescript Notes

1. The key diference is: Javascript uses "Dynamic Types" (Resolved at Runtimes), TypeScript uses "Static Types" (set during development)

2. Core Types
  - Number
  - String 
  - Boolean
  - Object
  - Array
  - Tuple (Array of especific value types and specific number of values , we could specified specific type to an especifict position to the array)
  - Enum (Human Reedable Identifiers)
  - Any (Any kink of value go back to Javascript)

3. Union Types use Pipes to define the diferente types of data that a variable should hold 
    ```typescript
    function combine(input1: number|string|boolean, imput2: string)  {
      
    }
    ```
4. The literar type lets you asign directly the values like constants options for teh variable to hold
    ```typescript
    {
      resultConversion: 'as-number' | 'as-text' 
    }
    ```
5. `type` alias let us define our custom types 
6. We can define the return type of the functions 
    ```typescript
      function add(n1:number , n2:number):number {
        return n1 + n2
      }

      // Not returns anything
      function print(n1:number ):void {
        console.log("Result " + n1);
      }
    ```
7. Function Types allow to specify wich type of function we want to use somewhere, we define them like arrow functions

8. `never` Type is a type ty explicitly say that a function will NEVER Return anything, not null not undefind nothing

9. sourceMap options Super usefull for debug it lets you add breackpoints for debuging