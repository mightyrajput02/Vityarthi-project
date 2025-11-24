# Vityarthi-project
New Repo
#Project: Text-Based Adventure RPG
# Author: Aryan Rajput
# Description: A game where players navigate rooms, collect items,
#              and try to defeat a boss to win.

import sys


def show_instructions():
    print("===================================================")
    print("           THE HAUNTED CASTLE ADVENTURE            ")
    print("===================================================")
    print("Objective: Escape the castle")
    print("You need to find the 'Sword' to defeat the Monster.")
    print("---------------------------------------------------")
    print("Commands:")
    print("  go [direction]  -> (north, south, east, west)")
    print("  get [item]      -> (e.g., get key)")
    print("  quit            -> End the game")
    print("===================================================")
    print()


def main():
    # (The Map)
    rooms = {
        'Great Hall': {
            'south': 'Bedroom',
            'north': 'Library',
            'east': 'Kitchen',
            'west': 'Garden'
        },
        'Library': {
            'south': 'Great Hall',
            'item': 'Map'
        },
        'Kitchen': {
            'west': 'Great Hall',
            'item': 'Potion'
        },
        'Bedroom': {
            'north': 'Great Hall',
            'east': 'Dungeon',
            'item': 'Sword'
        },
        'Dungeon': {
            'west': 'Bedroom',
            'item': 'Gold Coin'
        },
        'Garden': {
            'east': 'Great Hall',
            'boss': 'Monster'
        }
    }

    #STATE VARIABLES
    current_room = 'Great Hall'
    inventory = []

    show_instructions()

    # GAME CODE
    while True:
        print("\n-----------------------------------------")
        print(f"CURRENT LOCATION: {current_room}")
        print(f"INVENTORY: {inventory}")
        print("-----------------------------------------")

        if 'item' in rooms[current_room]:
            item_name = rooms[current_room]['item']
            print(f"  --> You see a {item_name} here!")
            print(f"      (HINT: Type 'get {item_name.lower()}' to pick it up)")
        else:
            print("  --> The room is empty of items.")

        available_exits = []
        for key in rooms[current_room]:
            if key != 'item' and key != 'boss':
                available_exits.append(key.title())

        print(f"  --> You can move to: {', '.join(available_exits)}")

        print("-----------------------------------------")

        if 'boss' in rooms[current_room]:
            boss_name = rooms[current_room]['boss']
            print(f"\nALERT: A wild {boss_name} blocks your path!")

            if 'Sword' in inventory:
                print(f"You draw your Sword and fight the {boss_name}...")
                print(f"You defeated the {boss_name}!")
                print("YOU WIN! You have escaped the castle!")
                break
            else:
                print("You have no weapon!")
                print(f"The {boss_name} attacks you.")
                print("GAME OVER.")
                break

        try:
            player_move = input("\nWhat do you want to do? > ").lower().split()
        except EOFError:
            break

        if len(player_move) == 0:
            continue

        action = player_move[0]

        if action == 'go':
            if len(player_move) < 2:
                print("Go where?")
                continue

            direction = player_move[1]

            if direction in rooms[current_room]:
                current_room = rooms[current_room][direction]
                print(f"You travel {direction}...")
            else:
                print("You can't go that way! Check the available exits.")

        elif action == 'get':
            if len(player_move) < 2:
                print("Get what?")
                continue

            item_wanted = player_move[1]

            if 'item' in rooms[current_room]:
                room_item = rooms[current_room]['item']
                if item_wanted == room_item.lower():
                    print(f"You picked up the {room_item}!")
                    inventory.append(room_item)
                    del rooms[current_room]['item']
                else:
                    print(f"There is no '{item_wanted}' here.")
            else:
                print("There is nothing here to take.")

        elif action == 'quit':
            print("Thanks for playing!")
            break

        else:
            print("I don't understand that command. Try 'go north' or 'get [item]'.")


if __name__ == "__main__":
    main()
