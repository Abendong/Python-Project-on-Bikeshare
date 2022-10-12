# Python-Project-on-Bikeshare
import time
import pandas as pd
import numpy as np

CITY_DATA = { 'chicago': 'chicago.csv',
              'new york city': 'new_york_city.csv',
              'washington': 'washington.csv' }

def get_filters():
    """
    Asks user to specify a city, month, and day to analyze.

    Returns:
        (str) city - name of the city to analyze
        (str) month - name of the month to filter by, or "all" to apply no month filter
        (str) day - name of the day of week to filter by, or "all" to apply no day filter
    """
    print('Hello! Let\'s explore some US bikeshare data!\n')
    # TO DO: get user input for city (chicago, new york city, washington). HINT: Use a while loop to handle invalid inputs
    while True:
        city = input('Please choose a city from chicago, new york city, washington: ').lower()
        if city not in CITY_DATA:
            print('choose a correct city name please')
        else:
            break
    
    # TO DO: get user input for month (all, january, february, ... , june)
    while True:
        month = input('Please choose any month from january to june, or type "all" to display all months: ').lower()
        months = ['january', 'february', 'march', 'april', 'may', 'june', 'all']
        if month != 'all' and month not in months:
            print('enter a valid full month name please')
        else:
            break

    # TO DO: get user input for day of week (all, monday, tuesday, ... sunday)
    while True:
        day = input('please enter a day of the week, or type "all" to display all days: ').lower()
        days = ['monday', 'tuesday', 'wednesday', 'thursady', 'friday', 'saturday', 'sunday', 'all']
        if day != 'all' and day not in days:
            print('enter a full valid day name please')
        else:
            break

    print('-'*40)
    return city, month, day


def load_data(city, month, day):
    """
    Loads data for the specified city and filters by month and day if applicable.

    Args:
        (str) city - name of the city to analyze
        (str) month - name of the month to filter by, or "all" to apply no month filter
        (str) day - name of the day of week to filter by, or "all" to apply no day filter
    Returns:
        df - Pandas DataFrame containing city data filtered by month and day
    """

    df = pd.read_csv(CITY_DATA[city])
    # convert the Start Time column to datetime
    df['Start Time'] = pd.to_datetime(df['Start Time'])

    # extract month and day of week from Start Time to create new columns
    df['month'] = df['Start Time'].dt.month
    df['day_of_week'] = df['Start Time'].dt.day_name

    # filter by month if applicable
    if month != 'all':
        # use the index of the months list to get the corresponding int
        months = ['january', 'february', 'march', 'april', 'may', 'june']
        month = months.index(month) + 1

        # filter by month to create the new dataframe
        df = df[df['month'] == month]

    # filter by day of week if applicable
    if day != 'all':
        # filter by day of week to create the new dataframe
        df = df[df['day_of_week'] == day.title()]

    return df
def display_data(df):
    """Displays raw bikeshare data."""
   
    x = 0
    user_request = input('Would youn like to display the first 5 rows of data? yes\no: ').lower()
    while True:
        if user_request == 'yes':
            print(df.head(5))
            user_request = input('Would youn like to display the next 5 rows of data? yes\no: ').lower()
            x += 5
            print(df.head(x))
        else:
            break
        #if df.iloc[0 : x].iloc[-1].name >= last_row_index_number :
            #break
        #else:
            #print('press anything except no to display more 5 row data or  press no to skip')
    #print('you displayed all row data')
            
def time_stats(df):
    """Displays statistics on the most frequent times of travel."""

    print('\nCalculating The Most Frequent Times of Travel...\n')
    start_time = time.time()

    # TO DO: display the most common month
    most_common_month = df['month'].mode()[0]
    months = ['january', 'february', 'march', 'april', 'may', 'june', 'all']
    name = months[most_common_month - 1]
    print('Most Common Month:', name)
                           

    # TO DO: display the most common day of week
    most_common_day = df['day_of_week'].mode()[0]
    print('Most Common Day:', most_common_day)

    # TO DO: display the most common start hour
    df['hour'] = df['Start Time'].dt.hour
    most_common_hour = df['hour'].mode()[0]
    print('Most Common Hour:', most_common_hour)

    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def station_stats(df):
    """Displays statistics on the most popular stations and trip."""

    print('\nCalculating The Most Popular Stations and Trip...\n')
    start_time = time.time()

    # TO DO: display most commonly used start station
    most_common_start = df['Start Station'].mode()[0]
    print('Most Commonly Used Start Station:', most_common_start)

    # TO DO: display most commonly used end station
    most_common_end = df['End Station'].mode()[0]
    print('Most Commonly used End Station:', most_common_end)

    # TO DO: display most frequent combination of start station and end station trip

    common_start_end = (df['Start Station'] + ' - ' + df['End Station']).mode()[0]
    print('Most Frequent Combination of Start and End Stations:', common_start_end)
    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def trip_duration_stats(df):
    """Displays statistics on the total and average trip duration."""

    print('\nCalculating Trip Duration...\n')
    start_time = time.time()

    # TO DO: display total travel time
    total_time = df['Trip Duration'].sum()
    print('Total Travel Time:', total_time)

    # TO DO: display mean travel time
    Average_time = df['Trip Duration'].mean()
    print('Average Travel Time:', Average_time)

    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def user_stats(df):
    """Displays statistics on bikeshare users."""

    print('\nCalculating User Stats...\n')
    start_time = time.time()

    # TO DO: Display counts of user types
    counts_user_type = df['User Type'].mode()[0]
    print('Counts of User types:', counts_user_type)

    # TO DO: Display counts of gender
    if 'Gender' in df:
        counts_gender = df['Gender'].mode()[0]
        print('Counts of Gender:', counts_gender)


    # TO DO: Display earliest, most recent, and most common year of birth
    if 'Birth Year' in df:
        earliest_birthyr = int(df['Birth Year'].min())
        print('Earliest Year of Birth:', earliest_birthyr)
        recent_birthyr = int(df['Birth Year'].max())
        print('Most Recent Year of Birth:', recent_birthyr)                   
        most_common_birthyr = int(df['Birth Year'].mode()[0])
        print('Most Recent Year of Birth:', most_common_birthyr)

        
    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)

   
    
    
def main():
    while True:
        city, month, day = get_filters()
        df = load_data(city, month, day)
        
        display_data(df)
        time_stats(df)
        station_stats(df)
        trip_duration_stats(df)
        user_stats(df)
        
        restart = input('\nWould you like to restart? Enter yes or no.\n')
        if restart.lower() != 'yes':
            break


if __name__ == "__main__":
	main()
