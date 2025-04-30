#!/bin/bash

# Function to generate a random sequence
generate_sequence() {
    local length=$1
    local sequence=""
    for ((i = 0; i < length; i++)); do
        rand_index=$((RANDOM % 36))
        chars="ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"
        sequence+=${chars:$rand_index:1}
    done
    echo "$sequence"
}

# Function to display ASCII visual representation
show_sequence() {
    echo "----------------------------------------"
    echo "  Remember this sequence: "
    echo "        $1       "
    echo "-----------------------------------------"
    sleep 5
    clear
}


get_user_input() {
    local sequence=$1
    local attempts=3
    local input
    while ((attempts > 0)); do
        echo -n "Enter the sequence (You have 10 seconds): "
        read -t 10 input
        if [[ "$input" == "$sequence" ]]; then
            echo "Correct! You have successfully recalled the sequence!"
            return 0
        else
            echo "Incorrect! You have $((--attempts)) attempts left."
        fi
    done
    return 1
}

# Game function
play_game() {
    local score=0
    local sequence_length=4
    while true; do
        sequence=$(generate_sequence $sequence_length)
        show_sequence "$sequence"
        if get_user_input "$sequence"; then
            ((score += 10))
            echo " Your current score: $score"
        else
            echo " Game Over! Your final score is: $score"
            echo "The correct sequence was: $sequence"
            break
        fi
        echo -n "Do you want to play again? (y/n): "
        read choice
        if [[ "$choice" != "y" ]]; then
            break
        fi
    done
}

# Main menu
echo "-----------------------------------------"
echo "   Welcome to the Memory Match Challenge! "
echo "-----------------------------------------"
echo "Can you recall the sequence before time runs out?"
play_game

