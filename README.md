# weather_recorder
import pandas as pd
from datetime import datetime

# Global data structures
weather_data = []
dates_recorded = set()

# Validate date format
def validate_date(date_str):
    try:
        datetime.strptime(date_str, '%Y-%m-%d')
        return True
    except ValueError:
        return False

# Add weather entry
def add_weather_data():
    date = input("Enter date (YYYY-MM-DD): ").strip()
    if not validate_date(date):
        print("❌ Invalid date format. Please use YYYY-MM-DD.")
        return

    if date in dates_recorded:
        print("⚠️ Data for this date already exists. Duplicate not added.")
        return

    try:
        temperature = float(input("Enter temperature (°C): ").strip())
        condition = input("Enter weather condition (e.g., Sunny, Rainy): ").strip()
    except ValueError:
        print("❌ Invalid temperature input.")
        return

    weather_data.append({
        'Date': date,
        'Temperature': temperature,
        'Condition': condition
    })
    dates_recorded.add(date)
    print("✅ Weather data added successfully.")

# View all data
def view_weather_data():
    if not weather_data:
        print("ℹ️ No data available.")
        return

    df = pd.DataFrame(weather_data)
    print("\n📋 Weather Data:")
    print(df.to_string(index=False))

# Show summary stats
def summarize_weather_data():
    if not weather_data:
        print("ℹ️ No data available for summary.")
        return

    df = pd.DataFrame(weather_data)
    avg_temp = df['Temperature'].mean()
    print(f"\n📊 Average Temperature: {avg_temp:.2f}°C")
    print("🌦️ Weather Condition Counts:")
    print(df['Condition'].value_counts())

# Export data to CSV
def export_to_csv():
    if not weather_data:
        print("ℹ️ No data to export.")
        return

    df = pd.DataFrame(weather_data)
    df.to_csv('weather_data.csv', index=False)
    print("📁 Data exported to weather_data.csv")

# Main menu
def main():
    while True:
        print("\n====== AgriWeather Recorder ======")
        print("1. Add Weather Data")
        print("2. View Weather Data")
        print("3. Summarize Data")
        print("4. Export to CSV")
        print("5. Exit")

        choice = input("Choose an option (1-5): ").strip()

        if choice == '1':
            add_weather_data()
        elif choice == '2':
            view_weather_data()
        elif choice == '3':
            summarize_weather_data()
        elif choice == '4':
            export_to_csv()
        elif choice == '5':
            print("👋 Exiting. Stay weather-aware!")
            break
        else:
            print("❌ Invalid choice. Please enter 1–5.")

if __name__ == "__main__":
    main()
