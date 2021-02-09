# Documentation

---

# Intro

C† is a Heisenberg-based hybrid **full-stack quantum programming language** created to help developers to transition to quantum algorithms **without having to learn quantum mechanics** or quantum physics properties in order to take advantage of its power.

The lower level language referred as **C†.low** or **C†.pulse** is the algorithm as a set of pulses to be sent to real quantum computer hardware or as a QASM algorithm for simulated quantum computers. Its main focus is to send the complex algorithm request as a simple sequence of pulses or gates to the quantum computer so it can be manipulated properly. Because it has no real interaction with the quantum computer, we call it **Heisenberg-based language** — only the gates sequence matters and then we collect the result by collapsing everything on the **measurement operator.**

The mid level language referred as **C†.mid,** **C†.gpp** or **C†.assembly** is a higher abstraction of code, **not** containing any direct **reference to quantum gates** or quantum properties. It is mostly intended to pave a way between a higher level language (such as C, C++, Python, Julia, JavaScript, Rust, Erlang, Lua, C†, etc.) and the C†.pulse.

It aims to **translate** function and class requests into set of **pulse instructions** based on the desired algorithm. Its second goal is to provide the right interpretation for each kind of algorithm. For this purpose, it calls an external language with high performance for data manipulation and analysis, such as **Julia**, to perform all the analysis on the sets of bit sequences from the measurements to give them the expected meaning, so **C†.assembly** can return a valid result for the high level language requester.

The high level language referred as **C†.high** is a completely abstract high level language the same way as high level languages are today. It connects the developer to the power of quantum computers through its familiar behavior from **functional** and **object-oriented languages** and connectivity to lower levels of C†.

In this documentation we will present the **C†.assembly** (from now on referred as **GPP**).

# **C†.assembly (GPP)**

## Syntax

Its syntax is fairy simple; it follows the same principle of **most spoken languages**: there is a subject that performs an action on an object. It is as follows: **subject: action [object]** . There must be a single subject that performs one or more actions on one or more objects. Each action may have one or more objects and each result may be placed on one or more attributes:  **subject: action [object] as attr** . There is no separator between actions other than the own logic of a language: after an action happens on an object, a next action may appear to happen on another object. In this sense, the developer is free to use the way the language fits best on their preference: indentation, one-liner, semicolon.

### subject

Subject must be a string not starting with special characters nor being one of the identifiers. It is always followed by a colon and an action with an object. When it is the main subject of a code, it starts as the first thing in the code. When it is a complementary subject, a **where** statement should be placed before it for every complementary subject. Example:

```
subj: action [obj]
			action [with subj2: action [obj]]
where
subj2: action [obj]
			 action [with subj3: action [obj]]
			 action [obj]
where
subj3: action [obj]
```

When placed as a object, it must come after either **with** or **if** statement. If there are more than one subject inside the object, they must be preceded by **&** as follows:

```
subj: action [with subj2: action [obj] & with subj3: action [obj]]
			action [obj & with subj2: action [obj] & obj obj]
where
subj2: action [obj]
where
	subj3: action [obj]
```

### action

Action is the link between the subject and the object by some specific means. Some actions perform on the object independently of the subject, others provide the strict link on them. An object set may contain one or more actions, in which are separated by a 'and' statement **&** as follows:

```
subj: action [obj]
		  action & action [obj]
		  action [with subj2: action [obj] action [obj]]
where
subj2: action [obj]
```

### object

Object contains either the data to be digested by the action or subject complements that define new layers for computing new actions on other objects. Data type can be numbers (integer, real, complex? - future implementation), string (between double quotes), hexadecimal (0x-), binary (0b-), attributes, subjects, arrays (to be implemented), dictionaries (to be implemented), logical statements, etc. Each new object is separated exclusively by space unless the previous one or the current one is a subject, placing a **&** between previous and current.

### attribute

Attribute is referred as a variable in other languages. To assign a value to a attribute, just do as follows:

```
subj: action [obj] as attr
```

The syntax **(before) as attr** literally means **attr = (before)** in other languages. There may be one or more attributes, as the example:

```
subj: action [obj1 obj2] as attr1 attr2
```

where the first object is mapped to the first attribute and so on. In case there are many objects and just one attribute, the whole set of objects will be assigned to that attribute.

When a subject complement receives an external attribute, it is defined as **attr**. It must be assigned to a local attribute before performing further actions on it.

### loop

Loop is a simple code  **_n...m**  placed after a wildcard variable, defined after the 'dollar' symbol **$**. In the definition, **n** and **m** can be integer or a set of data (array, string, etc). The clear example is as follows:

```
subj: action [obj$k]_n...m
			action [obj$k]_n...m as attr$k_n...m
		  action [with subj2: action [obj$k]_n...m]
			action [$k]_n...m%j
		  checks [(obj$k > obj|)_n...m]
```

The first value, **n** is the first item of the iteration, while **m** is the last. They can be applied to objects, attributes, objects inside objects and logical statements. In case of simple iteration with integers, it can be set as the examples: **_1...4** or **_5...40**. When the iterable is an array, it is defined as **_attr...** to go through all the values. The ***** can also be used on integers when the final value is not necessarily known. The **%** sign is the mod operation, where values will range from **n** to **m** and only be applicable when the value is divisible by **j**.

### logical operators

Logical operators are only available with **checks** action and they must be contained in between parenthesis, as follows:

```
subj: checks [(3 > 2) & (1 = 1) | ((3%2 = 0) & (4 - $k = 0&)_1...4)] as attr
			action [if attr: action [obj] else action [obj]]
```

Operations available are:

```
& ## for 'and'
| ## for 'or'
=, == ## for 'equal' sign
!=, =!=, ~=, <> ## for 'not equal' sign
> ## for 'bigger than' sign
< ## for 'smaller than' sign
>=, => ## for 'greater than' sign
<=, =< ## for 'less than' sign
% ## for 'mod'
```

Operations with loop such as **(4 - $k = 0&)_1...4** must be thought as **(4-1 = 0) & (4-2 = 0) & (4-3 = 0) & (4-4 = 0)**.

## Special Section about Inclusion & Learning

One particular goal of this language is to enable non-English speakers to program it and to learn and develop the future quantum applications for quantum computers and quantum networks. For that reason, a call is made for everyone willing to bring their own language to this programming language, so speakers of this language may be able to learn and develop programming skills on hybrid applications, empowering people and regions of the world other than the usual ones. Because the very structured and also flexible nature of this programming language, such endeavor can be achieved with the majority of spoken languages. Documentation should always come along with the language so no one is left behind.

## Types, Naming & Rules

### Types

There are the types: integer and real number types (with complex being implemented in next iteration), string, boolean, array (being implemented in next iteration), binary and hexadecimal.

- Everything that has no decimal values is considered integer, otherwise it is real.
- Everything that has double quotes (**"**) is considered string.
- Everything that is not empty array or **0** is considered as the boolean true, otherwise it is false.
- Everything that starts with **0b** is binary, while starting with **0x** is hexadecimal.

### Naming Conventions

Naming attributes has a convention to facilitate readability and translation. It is highly recommended to use variables as **v**, **var**, **attr**, **inst**, **data** or similar followed by an incremental number, as follows:

```
subj: action [obj] as v1
			action [obj] as v2
			action [obj] as v3
```

Naming loop variables is more flexible, since it will not be carried outside the scope of the loop. The only limitations, that is applied to every other naming entity, is:

- Not start with:
    - "[]&$:!<>=~|_.-+«»„“「」『』（）, digits or other punctuations
- Not end with:
    - :&$|!<>=~.+[]()_"«»„“「」『』（）and other punctuations
- Starting with ***** will indicate external attribute
- Ending with anything above will result in an interpretation of a new entity: string if double quotes, entity's entity if dot, loop variable if dollar symbol and so on.
- Except the cited above, any other Unicode character may be used for attributes, subjects and loop variables (but do keep in mind the common good practices for your chosen language).

### Rules

## List of Actions

- Actions with no attributes
    - **uses**

        **syntax**: uses [obj] | may contain loop

        **description**: sends one or more objects to the subject calling as a result (similar to return identifier on other languages)

        **equivalence**: Python: return var

        **examples**: uses [v1]

    - **returns**

        **syntax**: returns [obj] | may contain loop

        **description**: same as uses; same as return identifier on other languages

        **equivalence**: Python: return var

        **examples**: returns [v1]

    - **outputs**

        **syntax**: outputs [obj] | may contain loop

        **description**: the opposite of inputs; may be used as a "print" function if the first object is the queue for the monitor, may be a file, another queue, etc.

        **equivalence**: Python: print(obj2 ...) | with open(obj1, 'w') as f: f.write(obj2 ...) | etc

        **examples**: outputs ["queue1" 1 2 "hoi quantum"] outputs ["test.txt" "this is a file"]

    - **invokes**

        **syntax**: invokes [obj] | may contain loop

        **description**: calls object as a string representing a code file name to be executed independently

        **equivalence**: Python3: exec(open("path/file").read())

        **examples**: invokes ["factorial"]

    - **parallels**

        **syntax**: parallels [obj] | may contain loop

        **description**: runs every object concurrently (not defined yet if as a thread or process)

        **equivalence**: Python: with ThreadPoolExecutor() as exec: | with ProcessPoolExecutor() as exec:

        **examples**: parallels [with factorial: maps [10] & with factorial: maps [40]]

    - **processes**

        **syntax**: processes [obj] | may contain loop

        **description**: similar to parallels but with defined process

        **equivalence**:

        **examples**: processes [with factorial: maps [10] & with factorial: maps [40]]

    - **threads**

        **syntax**: threads [obj] | may contain loop

        **description**: similar to parallels but with defined threads

        **equivalence**:

        **examples**: threads [with factorial: maps [10] & with factorial: maps [40]]

    - **keeps**

        **syntax**: keeps [obj] | may contain loop

        **description**: is used to keep a object running indefinitely either by continuously looping it or by preventing it from closing

        **equivalence**: Python: while True: | (in case of concurrence, wait for futures answer)

        **examples**: keeps [with factorial: maps [10]] keeps & consumes [with x: maps [v1]]

    - **publishes**

        **syntax**: publishes [obj] | may contain loop

        **description**: sends an AMQP or other inter process communication between different programs and/or computers, usually first object as the queue and the second as the data to be sent

        **equivalence**: Python: ? (using RabbitMq for prototyping)

        **examples**: publishes ["queue1" {"cmd": "queue1", "data": [1 2 3 4]}]

- Actions with attributes
    - **loads**

        **syntax**: *loads [obj] as attr* | may contain loop

        **description**: load one or more files represented as objects (string) to one or more attributes

        **equivalence**: *Python*: with open(obj, 'r') as f: attr = f.read()

        **examples**: loads ["test.json"] as v1 loads ["test.yaml" "test.c†"] as v1 v2

    - **sets**

        **syntax**: *sets [obj] as attr* | may contain loop

        **description**: set one or more objects as one or more attributes

        **equivalence**: *Python*: attr = obj

        **examples**: sets [1 "hoi quantum" 0b1010 0xa] as v$n_1...4 sets [10] as v1

    - **maps**

        **syntax**: *action [with subj: maps [obj]]* | may contain attr and/or loop

        **description**: maps one or more objects to a subject

        **equivalence**: *Python*: subj(*obj)

        **examples**: applies [with factorial: maps [10] as v1]

    - **adds**

        syntax: adds [obj] as attr | may contain loop

        description: adds two or more objects together (numbers as sum, data structures and strings as append/join/merge)

        equivalence: Python: obj1 + obj2 | obj1.append(obj2) | obj1.update(obj2) | obj1.join(obj2) | ''.join([obj1, obj2])

        examples: adds [1 3] as v1 adds ["hoi" "quantum"] as v2 adds [["quantum" "here"] [10]] as v3

    - **multiplies**

        syntax: multiplies [obj] as attr | may contain loop

        description: multiplies two or more objects together (as * in Python for either numbers, strings or other data structures)

        equivalence: Python: obj1 * obj2

        examples: multiplies [10 20] as v1 multiplies ["a" 3] as v2 multiplies [["quantum"] 5] as v3

    - **powers**

        syntax: powers [obj] as attr | may contain loop

        description: gets the first object and elevates to the power of the second object and so forth

        equivalence: Python: obj1**obj2 | obj1**(obj2**(obj3))

        examples: powers [2 4] as v1

    - **divides**

        syntax: divides [obj] as attr | may contain loop

        description: divides two or more objects; if numbers, obj1/(obj2/(obj3...)), if the first is data structure or string and the second is integer, divide the first as ceiling integer number of times

        equivalence: Python: obj1/obj2 | [[obj1[i] for i in range(len(ojb1)) if (i % obj2) == r] for r in range(obj2)]

        examples: divides [20 5] as v1 divides [[1 2 3 4] 4] as v2

    - **executes**

        syntax: executes [obj] | may contatin loop and/or attr

        description: executes the sequence of objects as operating system command lines/inqueries

        equivalence: Python: attr = subprocess.run([obj1, obj2, ...], stdin=subprocess.PIPE, stdout=subprocess.PIPE); attr.stdout

        examples: executes ["ls" "-l"] as v1

    - **checks**

        syntax: checks [(obj)] as attr | may contain loop

        description: create one or more logical statements for conditional verification

        equivalence: Python: logical part of if statements

        examples: checks [(1 < 2) & (v$n > 5|)_1...4] as v1

    - **inputs**

        syntax: inputs [obj] as attr | may contain loop

        description: receives external data through pipe/fifo files (for inter process communication or peripherals data gathering)

        equivalence: Python: a = input() | another kind of inter process communication

        examples: inputs ["fifo_file"] as v1 inputs ["other_fifo_file" "pipe_file"] as v2 v3

    - **queues**

        syntax: queues [obj] as attr | may contain loop

        description: creates queues for internal or external communication where the first object is the name of the queue and one or more objects next are the data and/or parameters; kind of data structure to work with as well on the language

        equivalence: Python: ?

        examples: queues ["queue1" [1 2 3 4]] as v1

    - **dequeues**

        syntax: dequeues [obj] as attr | may contain loop

        description: reads data from object which is the queue's name

        equivalence: Python: ?

        examples: dequeues ["queue1"] as v1 dequeues ["queue2" "queue3" "queue4"] as v2 v3 v4

    - **consumes**

        examples: consumes ["queue1"] as v1 consumes ["queue2" "queue3"] as v2 v3

    - **reads**

        syntax: reads [obj] as attr | may contain loop

        description: reads data from other sources or to be a general action for retrieving external data?

        equivalence: Python: ?

        examples: reads [v1] as v2

    - **waits**

        **syntax**: waits [obj]

        **description**: can be used as a time.sleep() function where the object is the time (or times) if integer or real seconds and subjects to be executed after the previous object as number (time) passes

        **equivalence**: ?

        **examples**: waits [4] waits [0.5] waits [2 & with factorial: maps [10] & 4 & with factorial: maps [40]]
