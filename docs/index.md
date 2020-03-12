# Welcome

## Project layout



$$
\frac{n!}{k!(n-k)!} = \binom{n}{k}
$$


``` bash
#!/bin/bash
# parse json using "jq"

json=test.json

# loop over Type
Types=$(jq -r '.[] | .Type | @text' $json)
Types=( $Types )
for Type in ${Types[@]}; do
    # print targets for each  type
    #jq_cmd=$(echo "'map(select(.Type==\"$Type\"))|.[].Targets|@text'")
    #echo $jq_cmd
    Targets=$(jq --raw-output --arg Type "$Type" 'map(select(.Type == $Type)) 
	       | .[].Targets 
      	       | @sh'  $json | tr -d "'")
    Targets=( $Targets )
    echo "$Type ---> ${Targets[@]}"
done
``` 



!!! note
    Test
	
