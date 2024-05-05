---
layout: post
title:  "Understanding Object-Oriented Programming"
---

> Object-oriented programming is a programming paradigm that groups data and functions into a single 'object' to increase cohesion and reduce dependency.

When learning OOP, the most important thing is a shift in thinking. Martin Fowler says the best way to achieve this shift in thinking is to work for some time in an environment where OOP is well-structured. However, it is not easy to find such an environment because there are not many developers who properly understand OOP.

In this article, we will try to understand the essence of OOP by improving code from procedural to object-oriented style, and hopefully help a little in achieving this shift in thinking.

## 1. Introduction to Procedural Code

First, let's write a function that reads a document from a buffer and outputs it character by character.

```ts
/* Read from buffer */
function main() {
  const buffer = "Hello, World!";

  printDocument(buffer);
}

function printDocument(buffer: string) {
  for (let i = 0; i < buffer.length; i++) {
    const char = buffer.charAt(i);

    console.log(char);
  }
}
```

### 1.1. Adding Features to Procedural Code

Let's add a feature to read a document from a file and output it character by character.

```ts
function main() {
  /* Read from buffer */
  const buffer = "Hello, World!";
  printDocument(buffer);

  /* Read from file */
  const file = new File("test.txt");
  printDocument(file);
  file.close();
}

function printDocument(doc: string | File) {
  /* Read from buffer */
  if (typeof doc === "string") {
    for (let i = 0; i < doc.length; i++) {
      const char = doc.charAt(i);

      console.log(char);
    }
  } else if (doc instanceof File) {
    /* Read from file */
    let char = doc.getChar();

    while (char != EOF) {
      console.log(char);

      char = doc.getChar();
    }
  }
}
```

This is a typical procedural style. One characteristic of procedural code is that 'if' appears frequently.

## 2. Problems with Procedural Code

What if we need to add a feature to read from a REST API here? Not only the main() function but also the printDocument() function needs to be modified.

```ts
function main() {
  /* Read from buffer */
  const buffer = "Hello, World!";
  printDocument(buffer);

  /* Read from file */
  const file = new File("test.txt");
  printDocument(file);
  file.close();

  /* REST API request */
  const request = new HttpRequest("https://test.com/api");
  printDocument(request);
}

function printDocument(doc: string | File | HttpRequest) {
  /* Read from buffer */
  if (typeof doc === "string") {
    for (let i = 0; i < doc.length; i++) {
      const char = doc.charAt(i);

      console.log(char);
    }
  } else if (doc instanceof File) {
    /* Read from file */
    let char = doc.getChar();

    while (char != EOF) {
      console.log(char);

      char = doc.getChar();
    }
  } else if (doc instanceof HttpRequest) {
    /* REST API request */
    const body = doc.body();

    for (let i = 0; i < body.length; i++) {
      const char = body.charAt(i);

      console.log(char);
    }
  }
}
```

It's not a problem if it's just modifying the printDocument() function like now. But what if there are other functions besides printDocument()? The number of functions that need to be changed increases accordingly.

### 2.1. Problems in Nested Functions

Real projects often have longer and more complex code than this. In such an environment, it is not easy to find and modify all the related functions.

```ts
function main() {
    const buffer = "Hello, World!"
    printDocument(buffer)
    updateDocument(buffer, "new contents")
    clearDocument(buffer)

    const file = new File("test.txt")
    printDocument(file)
    updateDocument(file, "new contents")
    // To clear, you need to know the rule that it must be closed first.
    file.close()
    clearDocument(file)
}

function updateDocument(doc: string | File, contents: string) {
    if(typeof doc === 'string') {
        doc = contents
    }
    else if(doc instanceof File){
        doc.write(contents)
    }
    else if(doc instanceof HttpRequest){
        ...
    }
}

function clearDocument(doc: string | File) {
    if(typeof doc === 'string') {
        doc = ""
    }
    else if(doc instanceof File){
        doc.delete()
    }
    else if(doc instanceof HttpRequest){
        ...
    }
}

function printDocument(doc: string | File) {
    if(typeof doc === 'string') {
        for (let i = 0; i < doc.length; i++) {
            const char = doc.charAt(i)

            console.log(char)
        }
    }
    else if(doc instanceof File){
        let char = doc.getChar()

        while (char != EOF) {
            console.log(char)

            char = doc.getChar()
        }
    }
    else if(doc instanceof HttpRequest){
        ...
    }
}
```

'if' increases code complexity and makes development difficult. Removing the 'if' statements causes even bigger side effects.

## 3. How to Improve Procedural Code

### 3.1. Breaking Down Functions

As a way to avoid 'if', we can think of a method to break down printDocument().

```ts
function main() {
  const buffer = "Hello, World!";
  printBufferDocument(buffer);

  const file = new File("test.txt");
  printFileDocument(file);
}

function printBufferDocument(doc: string) {
  for (let i = 0; i < doc.length; i++) {
    const char = doc.charAt(i);

    console.log(char);
  }
}

function printFileDocument(doc: File) {
  let char = doc.getChar();

  while (char != EOF) {
    console.log(char);

    char = doc.getChar();
  }
}
```

At first glance, it seems like a good method.

But usually, functions are called nested like below.

```ts
function main() {
    const buffer = "Hello, World!"
    printBufferWeeklyReport(buffer)

    const file = new File("test.txt")
    printFileWeeklyReport(file)
}

function printBufferWeeklyReport(doc: string){
    /* code to generate report */
    ...

    printBufferDocument(report)
}

function printFileWeeklyReport(doc: File){
    /* code to generate report */
    ...

    printFileDocument(report)
}
```

The 'if' statements have definitely disappeared. However, in order to avoid 'if', the 'code to generate report' is repeated. If that's the case, it's better to use 'if'.

### 3.2. Passing Execution Code

The fundamental reason why 'if' cannot be removed in printDocument() is because the main() function only passes the data needed for printDocument(), but does not pass how to use that data.

So printDocument() has to determine which code to execute based on the type of data.

Then what if we pass the code to be executed together?

```ts
function main() {
  const buffer = "Hello, World!";
  let position = 0;
  const getCharFromBuffer = (doc: string) => {
    if (position == doc.length) return;

    return doc.charAt(position++);
  };

  printDocument(buffer, getCharFromBuffer);

  const file = new File("test.txt");
  const getCharFromFile = (doc: File) => {
    const char = doc.getChar();

    return char == EOF ? null : char;
  };

  printDocument(file, getCharFromFile);
  file.close();
}

function printDocument(doc: any, getChar: Func) {
  let char = getChar(doc);

  while (char != null) {
    console.log(char);

    char = getChar(doc);
  }
}
```

Although the main() function has become more complex, printDocument() no longer needs an 'if' statement and does not have to be changed no matter what format comes in.

Now, how can we clean up the main() function?

## 4. Object-Oriented Code

To clean up the main() function, we can use classes.

```ts
function main() {
  const bufferDocument = new BufferDocument("Hello, World!");
  printDocument(bufferDocument);

  const fileDocument = new FileDocument("test.txt");
  printDocument(fileDocument);
}

function printDocument(reader: DocumentReadable) {
  let char = reader.getChar();

  while (char != null) {
    console.log(char);
    char = reader.getChar();
  }

  reader.close();
}

interface DocumentReadable {
  getChar(): string | null;
  close(): void;
}

class BufferDocument implements DocumentReadable {
  private position: number;

  constructor(private buffer: string) {
    this.position = 0;
  }

  public getChar(): string | null {
    if (this.position === this.buffer.length) return null;

    return this.buffer.charAt(this.position++);
  }
}

class FileDocument implements DocumentReadable {
  private stream: ReadStream;

  constructor(filename: string) {
    this.stream = new File(filename);
  }

  public getChar(): string | null {
    const char = this.stream.getChar();
    return char !== EOF ? char : null;
  }

  public close(): void {
    this.stream.close();
  }
}
```

In the object-oriented printDocument() above, there is no 'if' statement to determine the document type. When an object is first created in main(), everything needed is determined. printDocument() only needs to use the given object.

> If you see 'if' statements in the code, think about whether this is procedural or not, and whether it can be improved to object-oriented.

Earlier, I said that procedural code only passes the data needed for printDocument(), not the specific method (function) to use that data. The object-oriented approach bundles data and the functions that use that data into an object and passes it. This is the biggest and most prominent difference.

## 5. Characteristics of Good Objects

Let's express the code as a class diagram.

{% plantuml %}
@startuml

interface DocumentReadable {
    +getChar() : string | null
    +close() : void
}

class BufferDocument {
    -buffer : string
    -position : number
    +constructor(buffer : string)
    +getChar() : string | null
}

class FileDocument {
    -stream : ReadStream
    +constructor(filename : string)
    +getChar() : string | null
    +close() : void
}

package printDocument <<Cloud>> {
}

printDocument --> DocumentReadable : uses
DocumentReadable <|.. BufferDocument
DocumentReadable <|.. FileDocument

@enduml
{% endplantuml %}

### 5.1. Separation of Concerns

The `printDocument()` function only uses the `DocumentReadable` interface.\
`BufferDocument` and `FileDocument` only provide their own functionality according to the `DocumentReadable` interface.

In other words, `printDocument()`, `BufferDocument`, and `FileDocument` only know the `DocumentReadable` interface and do not know about each other. This means that as long as the `DocumentReadable` interface does not change, there is no need to change each class or function.

To explain again, if the interface is not changed, no matter how each part is changed, it does not affect other parts. This is how object-oriented programming enables incremental development. This mechanism of separating interfaces and implementations naturally results in high cohesion and low coupling.

> If you are accustomed to procedural thinking, you may find it clumsy to aggregate functions in this way and focus only on that part. This appears not only in code but also in everyday work such as dividing tasks and collaborating.
>
> For example, a front-end developer and a back-end developer define a REST API and proceed with their own development. Then an error occurs in the backend that does not meet the previously defined REST API. When debugging, it is found that modifying the code in the backend is more complicated than changing the REST API and modifying the code in the frontend. The backend developer asks the frontend developer to modify the code, and the frontend developer gladly agrees.
>
> It is a big problem to abandon the principles set when designing the REST API. Moreover, by pulling the frontend into the backend's problem, the backend and frontend become that much more tightly coupled.
>
> You may wonder if such a small change is really such a big problem. It may not seem significant at the time of coding, but it becomes an obstacle when another developer tries to analyze the code after some time has passed.
>
> "Each person's work area must be strictly adhered to."
>
> Developers accustomed to procedural thinking sometimes perceive this statement as selfish and cold. However, this is an extremely technical approach.

### 5.2. Access Control

Functions can be seen as connected to each other through variables.

Let's think about global variables. It is often said that global variables should not be used. Because all functions that use a single global variable are connected to each other. In a situation where the source code exceeds 100,000 lines, how can you guarantee that other functions will behave according to the rules you think of?
This dramatically increases complexity.

> If you don't understand the extent of the impact when modifying code, you are almost certainly creating bugs.

In the diagram below, the read(), write(), change(), and reset() functions that use the 'count' variable are connected to each other. In particular, the change() and reset() functions that change the value have a direct impact on all other functions.

<!-- markdownlint-disable MD032 MD037 -->
{% plantuml %}
@startmindmap
* let count = 0
** function read() { return count }
** function write() { console.log(count) }
** function change() { count++ }
** function reset() { count = 0 }
@endmindmap
{% endplantuml %}

On the other hand, functions that use `buffer` or `position` of `BufferDocument` can only exist within the `BufferDocument` class.
This is because external access to the two properties (variables) is fundamentally blocked because they are private.

This is the reason why properties should not be directly exposed as public. If properties can be changed from the outside, it becomes impossible to know the extent of the impact when changing code related to the properties. Potentially, it becomes no different from a global variable.

## 6. Application of Object Orientation

The characteristics of object-oriented approach, which manages complexity through separation of concerns and minimizes the impact of changes, also influence other areas.

### 6.1. Microservices Architecture

MSA has many structural similarities to the object-oriented approach.

The core of object orientation is to bundle data and functions into a single object. In MSA, services also manage DB and API as one, and the internal implementation and DB of each service are not exposed to the outside. This becomes a great advantage for maintainability and scalability, while preventing changes within the service from affecting the outside.

{% plantuml %}
@startditaa
+----------------------+        +----------------------+
|         Class        |<------>|    Microservice      |
+----------------------+        +----------------------+
|                      |        |                      |
| + Property           |<------>| + Database           |
|                      |        |                      |
+----------------------+        +----------------------+
|                      |        | + API                |
| + Method()           |<------>|   + GET /resource    |
|                      |        |   + POST /resource   |
+----------------------+        +----------------------+
@endditaa
{% endplantuml %}

As you can see from the fact that OOP and MSA are structurally similar, if you do not properly understand OOP, it will be very difficult to understand and correctly design architectures like MSA.

These days, MSA seems to be in fashion. And many developers seem to be focused only on learning how to use MSA components such as API gateway, gRPC, and message brokers. However, the most important and fundamental thing is a deep understanding of OOP.

### 6.2. Agile Methodology

The way of working where related people such as designers, developers, and planners form a team and collaborate closely can be likened to the high cohesion and low coupling, which are the core principles of object-oriented programming (OOP). This approach plays an important role in increasing productivity and efficiency not only in software development but also in organization composition and teamwork.

The composition of an agile team itself is not the same structure as an object that combines data and functions into one. However, in an agile team, the planner produces data called requirements, and the developer implements them, so there is a similarity in that the subjects of data production and consumption are together. This can be seen in the same context as data (properties) and functions (methods) being closely related within an object.

On the other hand, the process of repeating incremental development, which is an important principle of agile methodology, is possible based on the separation of concerns in object-oriented design. Object-oriented programming models the system as interactions of independent objects, allowing each object to be developed and tested individually. This helps facilitate the small-scale development and feedback incorporation that agile methodology aims for. As a result, OOP is establishing itself as one of the main techniques underlying agile software development.

## 7. Conclusion

This article started like this:

> Object-oriented programming is a programming paradigm that groups data and functions into a single 'object' to increase cohesion and reduce dependencies.

That's right. Grouping data and functions into a single object is the core of object orientation.

However, many articles or videos explaining OOP describe what encapsulation, information hiding, polymorphism, and inheritance in OOP are. These four basic principles of OOP are just guidelines for grouping data and functions. The most basic and essential thing is the grouping of data and functions.

Then, can there be an object that only has data? Conversely, can there be an object that only has functions?

Even if you write code using the class syntax, it is not an object. An object only makes sense when it has both state (properties) and methods.

You may have felt like you understand something after reading this article. However, it will not be concrete until you actually try to change your own code to be object-oriented.

It takes a lot of thought and practice to determine what good code is.

OOP is at the core of modern development methodologies. If you don't have a deep understanding of OOP, it will be difficult to use modern development methods such as TDD, DDD, Agile, and MSA correctly, even if you study them. This is the reason why there are many projects that have applied MSA, but few success stories.

Design patterns are a collection of common patterns frequently used in object-oriented programming. We will cover this next time.
