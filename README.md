## Official repository of Transd programming language

##### Programming language in two C++ files (22 KLOCs).

* Static typing
* Data queries
* Classes
* Unicode
* First-class functions
* Generics
* Precise access control for class members
* More...

Example program:

```
#lang transd

MainModule: {
    tabfile: "/mnt/data/employees.csv",
    tabstr: "",

    _start: (λ 
//-- Read a CSV file into a string --
        (with fs FileStream()
            (open fs tabfile) (textin tabstr fs))

        (with tabl Table()
//-- Load table and build indexes --
            (load-table tabl tabstr)
            (build-index tabl "Age in Company (Years)")
            (build-index tabl "Salary")

//-- Do SELECT query
            (with rows (tsd-query tabl 
                    select: ["Name Prefix", "First Name", "Last Name",
                             "Age in Company (Years)", "Salary"]
                    as: [[String(),String(),String(),Double(),Int()]]
                    where: "\"Age in Company (Years)\" > 35.0 AND 
                        Salary < 43000"
                    sortby: "Salary")
//-- Print result
                (for row in rows do (lout row)))
        )
    )
}

OUTPUT:

[Drs., Cameron, Diggs, 36.35, 40119]
[Mr., Cory, Coyle, 37.62, 41078]
[Mr., Carol, Vangundy, 36.59, 41724]
[Mrs., Kristi, Beliveau, 38.39, 41796]
[Ms., Particia, Blair, 35.06, 41819]
[Mr., Wilber, Ransome, 37.67, 41994]
[Ms., Cathern, Pettit, 36.36, 42453]
[Mr., Lamar, Parson, 35.41, 42458]

Run-time:
 table loading: 20.41 sec;
 running query: 0.006 sec.
```
This program reads a 37 column table with 100,000 rows and makes a SELECT query on it.

You can run this example by following instructions [here](https://transd.org/perftest.html). 
