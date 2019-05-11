# ADTs

## Objects and Closures

- object is a collection of methods (code) and fields (data)
- closure is a function body (code) and environment (data)

```java
    class C {                           
        int x = 0;
        void set_x(int y) { x = y; }                // setter function
        int get_x() { return x; }                   // getter function
    }
    
    C c = new C();
    c.set_x(3);
    int y = c.get_x();
```
```ocaml
    let make () =                                   (* ocaml's version of a "constructor" *)
        let x = ref 0 in
        ( (fun y -> x := y), (fun () -> !x) )       (* setter and getter functions *)
    ;;
    
    let (set, get) = make ();;
    set 3;;                                         (* updates x since x is in the environment of the setter closure *)
    let y = get ();;
```
