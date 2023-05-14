## awk
---
### Syntax

    awk options 'selection _criteria {action }' input-file > output-file

### Need a date in leftmost column

    awk '{print d"\t"$1"\t"$2 }' d="$(date '+%F %T')" file

### Enumerate the lines of a file

    awk '{print NR,$1,$2 }' file

### Remove a header from a file

    awk 'NR>1' file

### Print the lines in a range

    awk 'NR>1 && NR < 4' file

### Remove all blank lines

    awk '1' RS='' file

### Sum the first column

    awk '{ SUM=SUM+$1 } END { print SUM }' FS=, OFS=, file

### Print the number characters in a line

    awk '{ print NR,"= " length($0) }' file
