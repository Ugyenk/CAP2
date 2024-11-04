#Programing the farming Drone# Programming the Farming Drone (Report)

## Introduction
The Farmer Was Replaced is a programming puzzle in which you automate the process of doing tasks (such as harvesting), that a farmer initially 
would do manually. It has them writing code to manipulate a robot that has taken the place of the farmer, getting it through a series of obstacles. The robot has to complete certain objectives in each level, like planting or harvesting crops, or moving around obstacles and cannot be controlled directly by the player.
The level progresses through tasks that demand writing efficient and bug-free code to complete them. It challenges the player to optimize the 
solution to various farming scenarios, and has reward credits for optimizing between an iterative and recursive solution, thus encouraging the player to think logically and learn coding.

# Table of Contents
- [Code Snippets and Explanation](#code-snippets-and-explanation)
- [Challenges and Learnings](#challenges-and-learnings)
- [References](#references)

# Code-Snippets-and-Explanation
Write and explain your code along with recordings.
## Step 1: Farming on 1 tile

**Code:**
While True:
   if can_harvest():
        harvest()
    else:
       do_a_flip()

**Explanation:**
This code runs an infinite loop.Each time, it checks if can_harvest() returns True.
- If True, it calls harvest() to collect resources.
- If False, it calls do_a_flip() as an alternative action.
In short: the code keeps checking if harvesting is possible and either harvests or does a flip based on the result, repeating forever.

**Demo:**
Video Demo:
![alt text](./1%20%20x%201%20.jpg)
**Notes**
- Using the code above I was able to get enough hay to unlock the tile
- These features were unlocked too:While Loops
                                   Speed Upgrades
                                   Expand Tile

                                   

## Step 2: Farming on 3x3 tile
**Code:**
clear()
move(South)
while True:
    for i in range(get_world_size()):
        move(North)
        if can_harvest():
            #Harvest
            harvest()
            #water Tank
            if num_items(Items.Water_Tank) < 100:
                trade(Items.Empty_Tank)
        #Watering
        if get_water() < 0.75:
            use_item()
        if num_items(Items.Hay) < 500:
            if get_ground_type() == Grounds.Soil:
                till()
            plant(Entities.Grass)
        elif num_items(Items.Wood) < 300:
            plant(Entities.Bush)
        else:
            if num_items(Items.Carrot_Seed) == 0:
                trade(Items.Carrot_Seed)
            if get_ground_type() == Grounds.Turf:
                till()
            plant(Entities.Carrots)
    move(East)


**Explanation:**
This code automates a farming bot to ensure it’s always harvesting, watering, and planting based on inventory needs
- Setup:The bot clears previous actions and moves south to start at a specific location.
- Loop Through Rows:The main loop makes the bot move north row by row, and at the end of each row, it moves east to start the next row. This 
                  helps cover the entire field.
- Harvesting:If the bot can harvest items, it does so.
- Watering:If the bot has fewer than 100 water tanks, it trades for more.
         If the water level is below 75%, it waters the plants.
- Planting:
- Grass: If there’s less than 500 Hay, it tills soil if needed and plants grass.
- Bushes: If there’s less than 300 Wood, it plants bushes.
- Carrots: If there are no carrot seeds, it trades for more. If on turf ground, it tills, then plants carrots.

**Demo:**
Video Demo:
![](./3%20x%203.jpg)
**Notes**
- Using the code above I was able to Farm and Harvest Hays,Bushes and Carrots.
- These features were unlocked too: 
.Operators
.plant()
.trade()
.till()
.water()

## Step 3: Farming On 5x5 tile
**Code**
# Main Code
def main():
    # Setup
    clear()
    move_south()
    
    # Variables
    resources = {
        "waterTank": 100,
        "hay": 20000,
        "wood": 10000,
        "carrot": 5000,
        "pumpkin": 1000
    }

    # Main loop
    for _ in range(get_world_size()):
        move_north()
        
        if can_harvest():
            harvest()
            watering(resources["waterTank"])
            planting(resources)
        elif get_ground_type() == Grounds.Soil:
            till()

        move_east()  # Next row


# Watering Function
def watering(waterTank):
    if num_items(Items.Water_Tank) < waterTank:
        trade(Items.Empty_Tank, 5)
    if get_water() < 0.75:
        use_item(Items.Water_Tank)


# Planting Function
def planting(resources):
    if num_items(Items.Sunflower_Seed) == 0:
        trade(Items.Sunflower_Seed, 5)
    if get_ground_type() == Grounds.Turf:
        till()
    plant(Entities.Sunflower)

    if num_items(Items.Hay) < resources["hay"]:
        till() if get_ground_type() == Grounds.Soil else None
        plant(Entities.Grass)
    elif num_items(Items.Wood) < resources["wood"]:
        plant(Entities.Tree if get_pos_x() % 2 == get_pos_y() % 2 else Entities.Bush)
    elif num_items(Items.Carrot) < resources["carrot"]:
        trade(Items.Carrot_Seed, 5) if num_items(Items.Carrot_Seed) == 0 else None
        till() if get_ground_type() == Grounds.Turf else None
        plant(Entities.Carrot)
    elif num_items(Items.Pumpkin) < resources["pumpkin"]:
        trade(Items.Pumpkin_Seed, 5) if num_items(Items.Pumpkin_Seed) == 0 else None
        till() if get_ground_type() == Grounds.Turf else None
        plant(Entities.Pumpkin)


# Placeholder functions to simulate the environment
def clear():
    print("Environment reset.")

def move_south():
    print("Moving south.")

def get_world_size():
    return 10

def move_north():
    print("Moving north.")

def can_harvest():
    return True

def harvest():
    print("Harvesting...")

def get_ground_type():
    return "Soil"

def till():
    print("Tilling...")

def move_east():
    print("Moving east.")

def num_items(item):
    return 100  # Example

def trade(item, quantity):
    print(f"Trading {quantity} of {item}.")

def get_water():
    return 0.5  # Example

def use_item(item):
    print(f"Using {item}.")

def plant(entity):
    print(f"Planting {entity}.")


# Simulated Enums for Items, Entities, and Grounds
class Items:
    Water_Tank = "Water Tank"
    Empty_Tank = "Empty Tank"
    Sunflower_Seed = "Sunflower Seed"
    Hay = "Hay"
    Wood = "Wood"
    Carrot = "Carrot"
    Carrot_Seed = "Carrot Seed"
    Pumpkin = "Pumpkin"
    Pumpkin_Seed = "Pumpkin Seed"

class Entities:
    Sunflower = "Sunflower"
    Grass = "Grass"
    Tree = "Tree"
    Bush = "Bush"
    Carrot = "Carrot"
    Pumpkin = "Pumpkin"

class Grounds:
    Soil = "Soil"
    Turf = "Turf"

# Run the main code
if __name__ == "__main__":
    main()
                                                      
# Challenges and Learnings
## Challenges
Discuss any significant challenges you faced while trying to solve the issues in the game.
Discuss any optimization methods you implemented to make the harvesting faster.
## Learnings
Note down what different algorithms and methods of programming you learnt (E.g: Given a number n, I learnt
an algorithm that makes the drone travel each tile one by one efficiently given it’s position)
## References
List any resources, articles, or libraries you used or referenced while working on this project.
1. [Resource 1 Title](URL)
2. [Resource 2 Title](URL)