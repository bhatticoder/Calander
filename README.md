#include <iostream>
#include <iomanip>
using namespace std;
//Prototypes
int GetFirstDay(int year);
int dayNumber(int day, int month, int year);
string getMonthName(int monthNumber);
void displayCalendar(int numdays, int startDay);
//Main Function
int main()
{
    int year, numdays, month = 1;
    char choice;
    do
    {
        cout << "Which function do you want to perform:" << endl;
        cout << "Enter 1 to display the calendar." << endl;
        cout << "Enter 2 to exit the program." << endl;
        cout << "Enter your choice: ";
        cin >> choice;
        switch (choice)
        {
        case '1':
            // Getting the user to give the year
            cout << "Enter year: ";
            cin >> year;
            cout << "\n\n";
            // Display the calendar for each month
            while (month <= 12)
            {
                // Print month name
                cout << getMonthName(month) << endl;
                cout << "S  M  T  W  T  F  S" << endl;
                cout << "-------------------" << endl;
                // Get the number of days in the month
                switch (month)
                {
                case 1: case 3: case 5: case 7: case 8: case 10: case 12:
                    numdays = 31;
                    break;
                case 4: case 6: case 9: case 11:
                    numdays = 30;
                    break;
                case 2:
                    numdays = (((year % 4 == 0) && (year % 100 != 0)) || (year % 400 == 0)) ? 29 : 28;
                    break;
                default:
                    numdays = 0; // Invalid month
                    break;
                }
                // Get the starting day of the month
                int startDay = dayNumber(1, month, year);
                // Call the function to display the calendar
                displayCalendar(numdays, startDay);
                cout << "-------------------" << endl << endl;
                month++;
            }
            month = 1; // Reset month for the next iteration
            break;
        case '2':
            cout << "Exiting the program." << endl;
            break;
        default:
            cout << "Invalid choice. Please enter 1 or 2." << endl;
            break;
        }
    } while (choice != '2');
    return 0;
}
int GetFirstDay(int year)
{
    int century = (year - 1) / 100;
    int y = (year - 1) % 100;
    int weekday = (((29 - (2 * century) + y + (y / 4) + (century / 4)) % 7) + 7) % 7;
    // 0 would be Sunday and 6 would be Saturday
    return weekday;
}
int dayNumber(int day, int month, int year)
{
    static int t[] = { 0, 3, 2, 5, 0, 3, 5, 1,
                    4, 6, 2, 4 };
    year -= month < 3;
    return (year + year / 4 - year / 100 +
        year / 400 + t[month - 1] + day) % 7;
}
string getMonthName(int monthNumber)
{
    string months[] = { "January", "February", "March",
                    "April", "May", "June",
                    "July", "August", "September",
                    "October", "November", "December"
    };
    if (monthNumber >= 1 && monthNumber <= 12)
    {
        return months[monthNumber - 1];
    }
    else
    {
        return "Invalid Month";
    }
}
void displayCalendar(int numdays, int startDay)
{
    // Print leading spaces based on the starting day
    for (int i = 0; i < startDay; i++)
    {
        cout << "   ";
    }
    // Print days of the month
    for (int day = 1; day <= numdays; day++)
    {
        cout << setw(2) << day << " ";
        if ((day + startDay) % 7 == 0)
        {
            cout << endl;
        }
    }
    // Move to the next line after the last day of the month
    if ((numdays + startDay) % 7 != 0)
    {
        cout << endl;
    }
}
