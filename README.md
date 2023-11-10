# Daily-temperature-with-dictionary-python
import os
import sys

def load_txt_file(file_name):
    file_content = []

    with open(os.path.join(sys.path[0], file_name), newline='', encoding="utf8") as file_obj:
        for line in file_obj.readlines():
            file_content.append(line.split())

    return file_content

def fahrenheit_to_celsius(fahrenheit: float) -> float:
    return (fahrenheit - 32) * 5 / 9


def average_temp_per_year(temperatures: dict) -> list:
    list_with_results = []
    dict_with_years = {}
    for item in temperatures:
        month = item[0]
        day = item[1]
        year = item[2]
        temperature = item[3]
        if year in dict_with_years.keys():
            dict_with_years[year].append(float(temperature))
        else:
            dict_with_years[year] = []
            dict_with_years[year].append(float(temperature))
    for year1, list_with_results in dict_with_years.items():
        average = sum(list_with_results) / len(list_with_results)
        list_with_results.append((year1, average))
    return list_with_results


def average_temp_per_month(temperatures: dict) -> list:
    list_with_results_for_month = []
    dict_with_months = {}
    for item in temperatures:
        month = item[0]
        temperature = item[3]
        if month in dict_with_months.keys():
            dict_with_months[month].append(float(temperature))
        else:
            dict_with_months[month] = []
            dict_with_months[month].append(float(temperature))
    for month, list_with_temperatures in dict_with_months.items():
        average = sum(list_with_temperatures) / len(list_with_temperatures)
        list_with_results_for_month.append((month, average))

    return list_with_results_for_month


def find_warmest_and_coldest_temperature_per_year(temperatures):
    dict_with_years = {}

    for item in temperatures:
        year = item[2]
        temperature = item[3]

        if year in dict_with_years:
            dict_with_years[year].append(float(temperature))
        else:
            dict_with_years[year] = [float(temperature)]

    result = {}

    for year, temperature_list in dict_with_years.items():
        warmest_temp = max(temperature_list)
        coldest_temp = min(temperature_list)
        result[year] = (warmest_temp, coldest_temp)
    return result   


if __name__ == "__main__":

    menu = True
    temperatures = load_txt_file("NLAMSTDM.txt")
    while menu:
        print("Menu:")
        print("[1] Print the average temperatures per year (Fahrenheit)")
        print("[2] Print the average temperatures per year (Celsius)")
        print("[3] Print the warmest and coldest year based on the average temperature")
        print("[4] Print the warmest month of a year based on the input year of the user (full month name)")
        print("[5] Print the coldest month of a year based on the input year of the user (full month name)")
        print("[6] Print a list of tuples where the first element of each tuple is the year and the second element of the tuple is a dictionary with months as the keys and the average temperature (in Celsius) of each month as the value")
        print("[0] Exit")

        choice = input("Enter your choice: ")

        if choice == "1":
            print(average_temp_per_year(temperatures))
        elif choice == "2":
            print(average_temp_per_month(temperatures))
        elif choice == "3":
            temperature_per_year = find_warmest_and_coldest_temperature_per_year(
                temperatures)
            if temperature_per_year:
                for year, (warmest, coldest) in temperature_per_year.items():
                    print(
                        f"Year {year}: Warmest Temperature = {warmest}°F, Coldest Temperature = {coldest}°F")
            else:
                print("No temperature data found.")
        elif choice == "4":
            year = int(input("Enter the year: "))
            pass
        elif choice == "5":
            year = int(input("Enter the year: "))
            pass
        elif choice == "6":
            pass
        elif choice == "0":
            menu = False
        else:
            print("Invalid choice. Please try again.")
