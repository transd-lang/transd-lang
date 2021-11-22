## Official repository of Transd programming language

##### Full-fledged programming language in two C++ files (20 KLOCs).

* Static typing
* Data queries
* Classes
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
                    where: "\"Age in Company (Years)\" > 30.0 AND 
                        Salary < 48000"
                    sortby: "Salary")
//-- Print result
                (for row in rows do (lout row)))
        )
    )
}

OUTPUT:

[Mrs., Sam, Flint, 30.12, 40359]
[Drs., Delma, Eberle, 34.04, 41097]
[Dr., Johnie, Amey, 36.6, 42940]
[Prof., Haywood, Nuss, 30.27, 43089]
[Mr., Vincenzo, Pryor, 35.93, 43538]
[Hon., Candace, Stamps, 32.87, 43642]
[Mrs., Emiko, Lange, 30.47, 43644]
[Mr., Demetrius, Simard, 36.13, 44285]
[Hon., Samual, Dowdy, 36.2, 44428]
[Mrs., Lorina, Shirley, 30.16, 45276]
[Hon., Arcelia, Slagle, 32.77, 45504]
[Ms., Laureen, Lecompte, 35.99, 45641]
[Prof., Wilford, Burk, 30.29, 46053]
[Ms., Liza, Chilton, 33.04, 46649]
[Mr., Hunter, Shine, 30.99, 46834]
[Mrs., Kelsey, Shaner, 32.01, 47528]
[Prof., Arlen, Neville, 30.01, 47686]

Run-time:
 table loading: 5 sec;
 running query: 0.034 sec.
```

In this program, a [sample dataset](https://eforexcel.com/wp/downloads-16-sample-csv-files-data-sets-for-testing/) was used, containing 10,000 records with 37 fields. To run this example, download that dataset, and change in the first line "Age in Company (Years)" to "Age in Company (Years):Double", and "Salary" to "Salary:Int".
