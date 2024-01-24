#!/bin/bash

# variable
alphabet="ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz123456789.,/?_-!: "
message="merci a TOPKLEAN de nous permettre de level up !!!!"
key="zerocool"

# position
p_index() {
    lettre="$1"
    alphabet="$2"
    index=0
    found=true
    for ((i = 0; i < ${#alphabet}; i++)); do
        if [ "${alphabet:$i:1}" = "$lettre" ]; then
            index=$i
            $found
            break
        fi
    done
    if [ "$found" ]; then
        echo "$index"
    else
        echo "l'index n'a pas été trouvé"  # la lettre n'est pas trouver
    fi
}
# chiffrer/déchiffrer un caractère avec la key
vigener_transform_car() {
    car="$1"
    key_car="$2"
    alphabet1="$3"
    operation="$4"
    # indice du caractere dans l'alphabet
    index_car=$(p_index "$car" "$alphabet1")
    # indice du caractere de la key dans l'alphabet
    index_key_car=$(p_index "$key_car" "$alphabet1")
    if [ "$index_car" -ge 0 ]; then
        # indice transforme en fonction de l'operation
        if [ "$operation" = "crypter" ]; then
            transforme_index=$(( (index_car + index_key_car) % ${#alphabet1} ))
        else
            echo "Opération non reconnue: $operation"
            exit 1
        fi
        transforme_car=${alphabet1:$transforme_index:1}
    else
        # si il y a pas le laisser inchanger
        transforme_car="$car"
    fi

    echo -n "$transforme_car"
}
# chiffrement/dechiffrement
vigener_transform() {
    input="$1"
    key="$2"
    alphabet1="$3"
    operation="$4"

    transforme_message=""
    key_length=${#key}

    for ((i = 0; i < ${#input}; i++)); do
        car="${input:$i:1}"
        key_car="${key:$(($i % $key_length)):1}"
        transforme_message+="$(vigener_transform_car "$car" "$key_car" "$alphabet1" "$operation")"
    done
    echo "$transforme_message"
}
# fonction
encrypte_message=$(vigener_transform "$message" "$key" "$alphabet" "crypter")
echo "Message clair: $message"
echo "KEY: $key"
echo "Message chiffré: $encrypte_message"
