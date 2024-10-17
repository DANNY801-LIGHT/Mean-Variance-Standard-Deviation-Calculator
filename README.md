import pandas as pd

def calculate_demographic_data():
    # Load the data
    df = pd.read_csv('adult.data.csv')

    # 1. How many people of each race are represented in this dataset?
    race_count = df['race'].value_counts()

    # 2. What is the average age of men?
    average_age_men = df[df['sex'] == 'Male']['age'].mean()

    # 3. What is the percentage of people who have a Bachelor's degree?
    percentage_bachelors = (df['education'] == 'Bachelors').mean() * 100

    # 4. What percentage of people with advanced education make more than 50K?
    higher_education = df['education'].isin(['Bachelors', 'Masters', 'Doctorate'])
    percentage_higher_education_rich = (df[higher_education & (df['salary'] == '>50K')].shape[0] / higher_education.sum()) * 100

    # 5. What percentage of people without advanced education make more than 50K?
    lower_education = ~higher_education
    percentage_lower_education_rich = (df[lower_education & (df['salary'] == '>50K')].shape[0] / lower_education.sum()) * 100

    # 6. What is the minimum number of hours a person works per week?
    min_work_hours = df['hours-per-week'].min()

    # 7. What percentage of the people who work the minimum number of hours per week have a salary of more than 50K?
    num_min_workers = df[df['hours-per-week'] == min_work_hours].shape[0]
    rich_percentage = (df[(df['hours-per-week'] == min_work_hours) & (df['salary'] == '>50K')].shape[0] / num_min_workers) * 100

    # 8. What country has the highest percentage of people that earn >50K and what is that percentage?
    country_earnings = df[df['salary'] == '>50K']['native-country'].value_counts()
    total_people_by_country = df['native-country'].value_counts()
    highest_earning_country_percentage = (country_earnings / total_people_by_country).max() * 100
    highest_earning_country = (country_earnings / total_people_by_country).idxmax()

    # 9. Identify the most popular occupation for those who earn >50K in India.
    top_IN_occupation = df[(df['native-country'] == 'India') & (df['salary'] == '>50K')]['occupation'].value_counts().idxmax()

    return {
        'race_count': race_count,
        'average_age_men': average_age_men,
        'percentage_bachelors': percentage_bachelors,
        'percentage_higher_education_rich': percentage_higher_education_rich,
        'percentage_lower_education_rich': percentage_lower_education_rich,
        'min_work_hours': min_work_hours,
        'rich_percentage': rich_percentage,
        'highest_earning_country': highest_earning_country,
        'highest_earning_country_percentage': highest_earning_country_percentage,
        'top_IN_occupation': top_IN_occupation
    }
