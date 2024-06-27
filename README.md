# Problem Statement  
AtliQ Grands, a prominent player in the Indian hospitality industry with over 20 years of experience and multiple five-star hotels, is currently facing a significant decline in market share and revenue within the luxury/business hotel category. This downturn is attributed to strategic moves by competitors and ineffective decision-making within the management team. To counteract this trend and regain their competitive edge, AtliQ Grands' managing director has decided to incorporate "Business and Data Intelligence" into their strategy. However, the lack of an in-house data analytics team poses a challenge in extracting actionable insights from their historical data. Therefore, AtliQ Grands' revenue management team has decided to engage a third-party service provider to leverage data analytics for informed decision-making and to drive revenue growth. The primary objective is to identify key factors contributing to the decline, optimize pricing strategies, enhance customer satisfaction, and ultimately restore AtliQ Grands' market position in the luxury/business hotel segment.    

## 1. ASK  
Revenue Management Team    

### Task:  
You are a data analyst who has been provided with sample data and a mock-up dashboard to work on the following task.
- Create the metrics according to the metric list.
- Create a dashboard according to the mock-up provided by stakeholders.
- Create relevant insights that are not provided in the metric list/mock-up dashboard.


## 2. PREPARE
### Data Storage:
The public dataset is completely available on the Code basis website platform where it stores and consolidates all available datasets for analysis. The specific individual datasets at hand can be obtained at this link below: https://codebasics.io/challenge/codebasics-resume-project-challenge

### Data Organized:
The dataset is taken from the AtliQ. Thanks to the AtliQ for providing datasets for public access which is a great learning asset - feel free to explore them. This dataset contains 5 csv file, 1 text file (meta_data) & mock_up_dashboard.  


## 3. PROCESS
### Tools Used:
- Microsoft Excel
- Power BI

### Data Used:
1. dim_date
2. dim_hotels
3. dim_rooms
4. fact_aggregated_bookings
5. fact_bookings

### About Data:  
Column Description for dim_date:
1. date: This column represents the dates present in May, June and July.
2. mmm yy: This column represents the date in the format of mmm yy (monthname year).
3. week no: This column represents the unique week number for that particular date.
4. day_type: This column represents whether the given day is Weekend or Weekeday.

Column Description for dim_hotels:
1. property_id: This column represents the Unique ID for each of the hotels.
2. property_name: This column represents the name of each hotel.
3. category: This column determines which class[Luxury, Business] a particular hotel/property belongs to. 
4. city: This column represents where the particular hotel/property resides in.

Column Description for dim_rooms:
1. room_id: This column represents the type of room[RT1, RT2, RT3, RT4] in a hotel.
2. room_class: This column represents to which class[Standard, Elite, Premium, Presidential] particular room type belongs.

Column Description for fact_aggregated_bookings:
1. property_id: This column represents the Unique ID for each of the hotels.
2. check_in_date: This column represents all the check_in_dates of the customers.
3. room_category: This column represents the type of room[RT1, RT2, RT3, RT4] in a hotel.
4. successful_bookings: This column represents all the successful room bookings that happen for a particular room type in that hotel on that particular date.
5. capacity: This column represents the maximum count of rooms available for a particular room type in that hotel on that particular date.

Column Description for fact_bookings:
1. booking_id: This column represents the Unique Booking ID for each customer when they booked their rooms.
2. property_id: This column represents the Unique ID for each of the hotels
3. booking_date: This column represents the date on which the customer booked their rooms.
4. check_in_date: This column represents the date on which the customer check-in(entered) at the hotel.
5. check_out_date: This column represents the date on which the customer check-out(left) of the hotel.
6. no_guests: This column represents the number of guests who stayed in a particular room in that hotel.
7. room_category: This column represents the type of room[RT1, RT2, RT3, RT4] in a hotel.
8. booking_platform: This column represents in which way the customer booked his room.
9. ratings_given: This column represents the ratings given by the customer for hotel services.
10. booking_status: This column represents whether the customer cancelled his booking[Cancelled], successfully stayed in the hotel[Checked Out] or booked his room but not stayed in the hotel[No show].
11. revenue_generated: This column represents the amount of money generated by the hotel from a particular customer.
12. revenue_realized: This column represents the final amount of money that goes to the hotel based on booking status. If the booking status is cancelled, then 40% of the revenue generated is deducted and the remaining is refunded to the customer. If the booking status is Checked Out/No show, then full revenue generated will goes to hotels.


## 4. ANALYZE
Data Analyzing  
Power BI was used to analyze data.

### Key Metrics:
#### Calculated Columns:  
| Calculated Column Name | Description / Purpose | DAX Formula | Table |
|-------------------------|-----------------------|-------------|-------|
| wn                      | To get the week number from the corresponding date | wn = WEEKNUM(dim_date[date]) | dim_date |
| day type                | Based on the feedback from stakeholder, we considered weekdays and weekends separately | day type = Var wkd = WEEKDAY(dim_date[date]) | dim_date |

#### Measures:
| Sno. | Measures                  | Description / Purpose | DAX Formula                                                                                                                           | Table                      |
|------|---------------------------|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------|----------------------------|
| 1    | Revenue                   | To get the total revenue_realized | Revenue = SUM(fact_bookings[revenue_realized])                                                                                         | fact_bookings              |
| 2    | Total Bookings            | To get the total number of bookings happened | Total Bookings = COUNT(fact_bookings[booking_id])                                                                                      | fact_bookings              |
| 3    | Total Capacity            | To get the total capacity of rooms present in hotels | Total Capacity = SUM(fact_aggregated_bookings[capacity])                                                                               | fact_aggregated_bookings   |
| 4    | Total Successful Bookings | To get the total successful bookings happened for all hotels | Total Successful Bookings = SUM(fact_aggregated_bookings[successful_bookings])                                                         | fact_aggregated_bookings   |
| 5    | Occupancy %               | Occupancy means total successful bookings happened to the total rooms available (capacity) | Occupancy % = DIVIDE([Total Successful Bookings],[Total Capacity],0)                                                                   | fact_aggregated_bookings   |
| 6    | Average Rating            | Get the average ratings given by the customers | Average Rating = AVERAGE(fact_bookings[ratings_given])                                                                                 | fact_bookings              |
| 7    | No of days                | To get the total number of days present in the data. In our case, we have data from May to July. So 92 days. | No of days = DATEDIFF(MIN(dim_date[date]),MAX(dim_date[date]),DAY) +1                                                                  | dim_date                   |
| 8    | Total cancelled bookings  | To get the "Cancelled" bookings out of all Total bookings happened | Total cancelled bookings = CALCULATE([Total Bookings],fact_bookings[booking_status]="Cancelled")                                       | fact_bookings              |
| 9    | Cancellation %            | Calculating the cancellation percentage. | Cancellation % = DIVIDE([Total cancelled bookings],[Total Bookings])                                                                   | fact_bookings              |
| 10   | Total Checked Out         | To get the successful 'Checked out' bookings out of all Total bookings happened | Total Checked Out = CALCULATE([Total Bookings],fact_bookings[booking_status]="Checked Out")                                            | fact_bookings              |
| 11   | Total no show bookings    | To get the "No Show" bookings out of all Total bookings happened ("No show" means those customers who neither cancelled nor attend to their booked rooms) | Total no show bookings = CALCULATE([Total Bookings],fact_bookings[booking_status]="No Show")                                           | fact_bookings              |
| 12   | No Show rate %            | Calculating the no show percentage. | No Show rate % = DIVIDE([Total no show bookings],[Total Bookings])                                                                     | fact_bookings              |
| 13   | Booking % by Platform     | To show the percentage contribution of each booking platform for bookings in hotels. We have booking platforms like makeyourtrip, logtrip, tripster etc) | Booking % by Platform = DIVIDE([Total Bookings], CALCULATE([Total Bookings], ALL(fact_bookings[booking_platform]))) * 100             | fact_bookings              |
| 14   | Booking % by Room class   | To show the percentage contribution of each room class over total rooms booked. We have room classes like Standard, Elite, Premium, Presidential. | Booking % by Room class = DIVIDE([Total Bookings], CALCULATE([Total Bookings], ALL(dim_rooms[room_class]))) * 100                      | fact_bookings, dim_rooms   |
| 15   | ADR                       | Calculate the ADR(Average Daily rate). It is the ratio of revenue to the total rooms booked/sold. It is the measure of the average paid for rooms sold in a given time period | ADR = DIVIDE([Revenue], [Total Bookings], 0)                                                                                           | fact_bookings              |
| 16   | Realisation %             | Calculate the realisation percentage. It is nothing but the successful "checked out" percentage over all bookings happened. | Realisation % = 1- ([Cancellation %] + [No Show rate %])                                                                               | fact_bookings              |
| 17   | RevPAR                    | Calculate the RevPAR(Revenue Per Available Room). RevPAR represents the revenue generated per available room, whether or not they are occupied. | RevPAR = DIVIDE([Revenue], [Total Capacity])                                                                                           | fact_bookings, fact_agg_bookings |
| 18   | DBRN                      | Calculate DBRN(Daily Booked Room Nights). This metric tells on average how many rooms are booked for a day considering a time period. | DBRN = DIVIDE([Total Bookings], [No of days])                                                                                          | fact_bookings, dim_date    |
| 19   | DSRN                      | Calculate DSRN(Daily Sellable Room Nights). This metric tells on average how many rooms are ready to sell for a day considering a time period. | DSRN = DIVIDE([Total Capacity], [No of days])                                                                                          | fact_agg_bookings, dim_date |
| 20   | DURN                      | Calculate DURN(Daily Utilized Room Nights). This metric tells on average how many rooms are successfully utilized by customers for a day considering a time period. | DURN = DIVIDE([Total Checked Out], [No of days])                                                                                       | fact_bookings, dim_date    |
| 21   | Revenue WoW change %      | To get the revenue change percentage week over week. Here, revcw for current week, revpw for previous week. | Revenue WoW change % = Var selv = IF(HASONEFILTER(dim_date[wn]), SELECTEDVALUE(dim_date[wn]), MAX(dim_date[wn])) var revcw = CALCULATE([Revenue], dim_date[wn] = selv) var revpw = CALCULATE([Revenue], FILTER(ALL(dim_date), dim_date[wn] = selv-1)) return DIVIDE(revcw, revpw, 0) - 1 | dim_date                   |
| 22   | Occupancy WoW change %    | To get the occupancy change percentage week over week. Here, revcw for current week, revpw for previous week. | Occupancy WoW change % = Var selv = IF(HASONEFILTER(dim_date[wn]), SELECTEDVALUE(dim_date[wn]), MAX(dim_date[wn])) var revcw = CALCULATE([Occupancy %], dim_date[wn] = selv) var revpw = CALCULATE([Occupancy %], FILTER(ALL(dim_date), dim_date[wn] = selv-1)) return DIVIDE(revcw, revpw, 0) - 1 | dim_date                   |
| 23   | ADR WoW change %          | To get the ADR(Average Daily rate) change percentage week over week. Here, revcw for current week, revpw for previous week. | ADR WoW change % = Var selv = IF(HASONEFILTER(dim_date[wn]), SELECTEDVALUE(dim_date[wn]), MAX(dim_date[wn])) var revcw = CALCULATE([ADR], dim_date[wn] = selv) var revpw = CALCULATE([ADR], FILTER(ALL(dim_date), dim_date[wn] = selv-1)) return DIVIDE(revcw, revpw, 0) - 1 | dim_date                   |
| 24   | RevPAR WoW change %       | To get the RevPAR(Revenue Per Available Room) change percentage week over week. Here, revcw for current week, revpw for previous week. | RevPAR WoW change % = Var selv = IF(HASONEFILTER(dim_date[wn]), SELECTEDVALUE(dim_date[wn]), MAX(dim_date[wn])) var revcw = CALCULATE([RevPAR], dim_date[wn] = selv) var revpw = CALCULATE([RevPAR], FILTER(ALL(dim_date), dim_date[wn] = selv-1)) return DIVIDE(revcw, revpw, 0) - 1 | dim_date                   |
| 25   | Realisation WoW change %  | To get the Realisation change percentage week over week. Here, revcw for current week, revpw for previous week. | Realisation WoW change % = Var selv = IF(HASONEFILTER(dim_date[wn]), SELECTEDVALUE(dim_date[wn]), MAX(dim_date[wn])) var revcw = CALCULATE([Realisation %], dim_date[wn] = selv) var revpw = CALCULATE([Realisation %], FILTER(ALL(dim_date), dim_date[wn] = selv-1)) return DIVIDE(revcw, revpw, 0) - 1 | dim_date                   |
| 26   | DSRN WoW change %         | To get the DSRN(Daily Sellable Room Nights) change percentage week over week. Here, revcw for current week, revpw for previous week. | DSRN WoW change % = Var selv = IF(HASONEFILTER(dim_date[wn]), SELECTEDVALUE(dim_date[wn]), MAX(dim_date[wn])) var revcw = CALCULATE([DSRN], dim_date[wn]=selv) var revpw=CALCULATE([DSRN],FILTER(ALL(dim_date),dim_date[wn]= selv-1)) return DIVIDE(revcw,revpw,0)-1 | dim_date |


## 5. SHARE  













