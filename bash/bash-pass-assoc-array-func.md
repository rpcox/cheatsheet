## pass an associative array to a function


    #!/bin/bash

    declare -A MC
     MC[Suzuki]="GSX-R1000"
     MC[Kawasaki]="Ninja 1000SX"
     MC[Yamaha]="YZF-R1"

    declare -A CAR
     CAR[Porshe]="911 Carrera GTS"
     CAR[Aston-Martin]="Valhalla"
     CAR[Bugatti]="Venyon"

    declare -a sorted_keys

    function SortKeys {
    	local -n array_ref=$1
    	sorted_keys=$(echo "${!array_ref[@]}" | tr ' ' '\n' | sort)
    }

    function PrintArray {
    	local -n array_ref=$1
    	local -n keys_ref=$2
    	for k in ${keys_ref}; do
    		printf "%12s %s\n" "$k" "${array_ref[$k]}"
    	done

    }

    SortKeys MC
    PrintArray MC sorted_keys
    echo
    unset sorted_keys
    SortKeys CAR
    PrintArray CAR sorted_keys


### output


        Kawasaki Ninja 1000SX
          Suzuki GSX-R1000
          Yamaha YZF-R1

    Aston-Martin Valhalla
         Bugatti Venyon
          Porshe 911 Carrera GTS
