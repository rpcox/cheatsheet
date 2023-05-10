## bash associative array
---

### create

    $ declare -A MYARRAY     # Explicit creation. declare an empty associative array
    $ AA[foo]=bar            # Implcit creation. Associate a key (foo) with a value (bar)

#### declare and initialize at once

    $ declare -A AA=( [yamaha]=fz07 [suzuki]=gsxr750 [kawasaki]=z10 )

    $ declare -A AA
    AA=( [yamaha]=fz07 [suzuki]=gsxr750 [kawasaki]=z10 )

### test for key presence

    AA[yamaha]=fz07         # create one association
    echo ${AA[ducati]}      # no association.  returns ""

### test for value presence

    $ declare -A AA=( [yamaha]=fz10 [suzuki]=gsxr750 [kawasaki]=z10 )
    $ echo "${!AA[@]}"      # print all keys

    $ if [ ${AA[yamaha]+_} ]; then echo present; else echo not present; fi
    present
    $ if [ ${AA[ducati]+_} ]; then echo present; else echo not present; fi
    not present

### delete the associative array

    $ declare -A AA=( [yamaha]=fz10 [suzuki]=gsxr750 [kawasaki]=z10 )
    $ unset AA
    $ echo ${AA[@]}         # nothing printed

### loop through keys and values

    $ declare -A AA=( [yamaha]=fz10 [suzuki]=gsxr750 [kawasaki]=z10 )
    $ for k in "${!AA[@]}"; do echo "k=$k v=${AA[$k]}"; done
    k=yamaha v=fz10
    k=kawasaki v=z10
    k=suzuki v=gsxr750

### delete keys

    $ declare -A AA=( [yamaha]=fz10 [suzuki]=gsxr750 [kawasaki]=z10 )
    $ for k in "${!AA[@]}"; do echo "k=$k v=${AA[$k]}"; done
    k=yamaha v=fz10
    k=kawasaki v=z10
    k=suzuki v=gsxr750
    $ unset AA[yamaha]
    $ for k in "${!AA[@]}"; do echo "k=$k v=${AA[$k]}"; done
    k=kawasaki v=z10
    k=suzuki v=gsxr750

### loop through w/ sorted keys

    $ declare -A AA=( [yamaha]=fz10 [suzuki]=gsxr750 [kawasaki]=z10 )
    $ sorted_keys=$(echo "${!AA[@]}" | tr ' ' '\n' | sort)
    $ for k in $sorted_keys; do echo "k=$k v=${AA[$k]}"; done
    k=kawasaki v=z10
    k=suzuki v=gsxr750
    k=yamaha v=fz10

### loop through w/ sorted values

    $ declare -A AA=( [yamaha]=fz10 [suzuki]=gsxr750 [kawasaki]=z10 )
    $ sorted_values=$(echo ${AA[@]} | tr ' ' '\n' | sort)
    $ for v in $sorted_values; do for k in $sorted_keys; do if [ ${AA[$k]} == $v ]; then echo "k=$k v=${AA[$k]}"; fi; done; done
    k=yamaha v=fz10
    k=suzuki v=gsxr750
    k=kawasaki v=z10


