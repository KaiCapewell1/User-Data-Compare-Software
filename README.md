import os

class Person:
    filePath = "database.txt"

    #-------------------------------------------------Declaring the varibles--------------------------------------------------------------------#
    def __init__(self, name, age, height, weight, stepsToday, waterIntake, heartBeat):
        self.name = name
        self.age = age
        self.height = height
        self.weight = weight
        self.stepsToday = stepsToday
        self.waterIntake = waterIntake
        self.heartBeat = heartBeat

    @staticmethod
    #-------------------------------------------------entering the inputs--------------------------------------------------------------------#
    def inputs(prompts, type):
        while True:
            try:

                user_input = type(input(prompts))

                # If input is an int or float, check if it's greater than 0
                if type in [int, float]:
                    if user_input <= 0:
                        print("Error: Input must be greater than 0.")
                        continue

                # If input is a string, check if it's non-empty
                elif type == str:
                    if not user_input.strip():
                        print("Error: Input cannot be empty.")
                        continue

                return user_input

            except ValueError:
                print(f"Error: Invalid input. Please enter a valid {type.__name__}.")

    #-------------------------------------------calculating the BMI----------------------------------------------------#

    def BMICal(weight, height):
        bmi = round(weight / (height ** 2), 1)
        return bmi

    @staticmethod
    #------------------------------------------------Saving to a file---------------------------------------------------------------------#
    def SaveToFile(name, age, height, weight, stepsToday, waterIntake, heartBeat, bmi):
        with open("database.txt", "a") as f:
            f.write(f"Name: {name}, Age: {age}, Height: {height}, Weight: {weight}, Steps Today: {stepsToday}, "
                    f"Water Intake: {waterIntake}L, Heartbeat: {heartBeat} BPM, BMI: {bmi}\n")
            f.flush()
    #------------------------------------------------pulling infomation from a file(not working)---------------------------------------------------------------------#
    def compareFile(filePath, name):
        userInputs = []  # List to store inputs for the given name
        try:
            with open(filePath) as file:
                lines = file.readlines()

                # Collect all inputs for the given name
                for line in lines:
                    if name.lower() in line.lower():  # Check for case-insensitive match
                        userInputs.append(line.strip())  # Store the line if it matches

                # Check if there are at least two previous inputs
                if len(userInputs) >= 2:
                    previousInput = userInputs[-2]
                    CurrentInput = userInputs[-1]



                    print(f"previous Input: {previousInput}")
                    print(f"Current Input: {CurrentInput}")

                # If only one input exists, just print that
                elif len(userInputs) == 1:
                    print(f"{userInputs[-1]} Has been added to the database")
                else:
                    print("User not found in file.")  # If no inputs were found for the name

        except FileNotFoundError:
            print("Error: File not found")

    #-----------------------------------the Main Functiom-------------------------------------------------------#
def main():
    while True:

        name = Person.inputs("Name: ", str)
        age = Person.inputs("Age: ", int)
        height = Person.inputs("Height: ", float)
        weight = Person.inputs("Weight: ", float)
        stepsToday = Person.inputs("Steps Today: ", int)
        waterIntake = Person.inputs("Water Intake Today: ", float)
        heartBeat = Person.inputs("BPM: ", int)

        #-----------------------------------creates a person object-------------------------------------------------------#
        user = Person(name, age, height, weight, stepsToday, waterIntake, heartBeat)
        #print(f"Person's name: {user.name}, Age: {user.age}, Height: {user.height}, Weight: {user.weight}, Steps Today: {user.stepsToday}, Water Intake: {user.waterIntake}L, Heartbeat: {user.heartBeat} BPM")

        bmiprint = Person.BMICal(weight, height)

        #-----------------------------------Input into text-------------------------------------------------------#
        Person.SaveToFile(name, age, height, weight, stepsToday, waterIntake, heartBeat, bmiprint)

        Person.compareFile(Person.filePath, name)
        print("")


if __name__ == "__main__":
    main()
