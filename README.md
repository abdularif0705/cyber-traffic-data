## Cyber Traffic Data 🚦

This project was created for the [BorderHacks 2021 Traffic Data Challenge](https://devpost.com/software/cyber-traffic-data).

## Inspiration 💡

Our goal is to help local Windsorites understand their traffic patterns better, aiming to make the city safer. We leveraged the City of Windsor’s OpenData API to provide valuable traffic insights.

## Features 👀

- **API Access**: Users can access Windsor’s OpenData API and view traffic data based on their input.
- **Visualization**: Traffic data is visualized using Google Maps, providing an intuitive and interactive experience.

## How We Built It 🛠️

- **Frontend**: Developed using HTML, CSS, and JavaScript, incorporating the Google Maps API for visualization.
- **Backend**: Machine learning models were built using Python to analyze and predict traffic patterns.

## Machine Learning Details 🤖

### Data Collection and Preparation
We fetched traffic data for the years 2020 and 2021 from the City of Windsor’s OpenData API. The data was then cleaned and transformed for analysis:
```python
# Fetching traffic data from the API
start_date = date(2020, 1, 1)
end_date = date(2020, 12, 31)
date_diff = end_date - start_date
for i in range(date_diff.days + 1):
    dates = start_date + td(days=i)
    url = f"https://opendata.citywindsor.ca/api/traffic?date={dates}&intersectionId=1&start_time=00%3A00&end_time=23%3A59"
    if i == 0:
        df = requests.get(url=url, verify=False)
data_2020 = pd.DataFrame.from_dict(df.json(), orient='columns')

# Converting JSON data to DataFrame
df1 = pd.DataFrame(data_2020['traffic'].values.tolist())
data_2020 = pd.concat([data_2020, df1], axis=1)
```

### Feature Engineering
We added new time-related columns to the DataFrame to capture more information:
```python
vehicle['timeStamp'] = pd.to_datetime(vehicle['timeStamp'])
vehicle['day_of_week'] = vehicle['timeStamp'].dt.dayofweek
vehicle['day'] = vehicle['timeStamp'].dt.day
vehicle['day_of_year'] = vehicle['timeStamp'].dt.dayofyear
vehicle['year'] = vehicle['timeStamp'].dt.year
vehicle['month'] = vehicle['timeStamp'].dt.month
vehicle['hours'] = vehicle['timeStamp'].dt.hour
```

### Model Training
We used the XGBoost regression model to predict traffic patterns:
```python
# Splitting the data into training and testing sets
X = hour_count.drop(['Vehicles'], axis=1)
y = hour_count.Vehicles
xtrain, xtest, ytrain, ytest = train_test_split(X, y, test_size=0.2, shuffle=True)

# Training the XGBoost model
xgbr = xgb.XGBRegressor(verbosity=0)
xgbr.fit(xtrain, ytrain)
```

### Model Evaluation
The model's performance was evaluated using accuracy scores and visualizations:
```python
# Calculating training and testing accuracy
train_score = xgbr.score(xtrain, ytrain)
test_score = xgbr.score(xtest, ytest)
print(f"Training score: {train_score}")
print(f"Testing score: {test_score}")

# Making predictions and plotting results
ypred = xgbr.predict(xtest)
plt.subplots(figsize=(21, 6))
plt.plot(range(len(ytest)), ytest, label="original")
plt.plot(range(len(ytest)), ypred, label="predicted")
plt.legend()
plt.show()
```

### Cross-Validation and Unseen Data
We performed cross-validation to ensure the model's robustness and evaluated it on unseen data:
```python
# Cross-validation
scores_xg = cross_val_score(xgbr, X, y, cv=5)
print(f"Cross-validation scores: {scores_xg}")
print(f"Mean accuracy: {scores_xg.mean()}")

# Evaluating on unseen data
score_unseen = xgbr.score(X_unseen, y_unseen)
print(f"Accuracy on unseen data: {score_unseen}")
```

## Accomplishments We're Proud Of 🏆

- **Learning and Innovation**: We explored new machine learning algorithms and APIs, expanding our technical skills.
- **Impactful Solution**: Created a tool that can potentially improve traffic safety and efficiency in Windsor.

## What We Learned 🧠

- **Machine Learning**: Implemented and experimented with various machine learning algorithms.
- **API Integration**: Gained experience in querying and integrating multiple APIs, including Google Maps and Windsor’s OpenData API.
- **Collaboration**: Worked effectively as a team, combining our diverse skill sets to build a comprehensive solution.

## Challenges We Faced 🚧

- **Backend Integration**: We faced difficulties in building and integrating the backend with our predictive model.

## What's Next for Windsor Traffic Data ⏰

- **Social Media Integration**: We plan to use social media platforms like Twitter to enhance traffic predictions. By scraping tweets from the past 24 hours in Windsor, we aim to identify traffic incidents and use a classification model to predict their impact.
- **Real-Time Notifications**: Implement features to notify users of accidents and other traffic disruptions in real-time.

## Team Members 👥

- **Abdul Arif**: Full stack developer, worked on the entire website and machine learning models. First-time implementation of Google Maps API in JavaScript.
- **Vidhi Gupta**: Machine learning specialist, focused on developing and implementing predictive models using Python.
- **Harsh Tiwari**: Frontend developer, designed forms, styled pages, and resized maps for better user experience.

## Technologies Used 🛠️

- **Languages**: HTML, CSS, JavaScript, Python
- **APIs**: Google Maps API, Windsor’s OpenData API
- **Libraries**: Machine learning libraries in Python

## How to Use 📖

1. **Access the Site**: Visit our website to start exploring traffic data.
2. **Input Data**: Enter your desired parameters to query the Windsor OpenData API.
3. **View Results**: Visualize the traffic data on an interactive map.

## Future Plans 🚀

- **Enhanced Predictions**: Improve our machine learning models for more accurate traffic predictions.
- **User Notifications**: Develop a system to alert users about real-time traffic incidents.

## Conclusion 🎉

Cyber Traffic Data is a step towards making Windsor a safer and more efficient city by providing valuable traffic insights through innovative technology.
