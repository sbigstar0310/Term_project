# Term_project
KAIST IE221 Term project

# **Train Interval Optimization Problem**

---

## **1. Problem Overview**

The goal is to optimize the train schedule for a set of stations, minimizing operational costs while balancing passenger waiting times and discomfort. The problem involves selecting the optimal number of trains per hour ($n_t$) and corresponding train intervals ($x_t$), considering passenger boarding and alighting data and train capacity.

---

## **2. Basic Assumptions**

1. **Passenger Flow:**
    - Boarding and alighting passenger numbers are known for each station.
    - Passenger flow at each station is calculated based on the difference between boarding and alighting, adjusted for the previous station’s passenger count.
2. **Train Capacity:** Maximum number of passengers held by train.
3. **Discomfort Index:** Passenger discomfort is modeled as proportional to the total number of passengers divided by the train capacity, scaled by a discomfort constant.
4. **Interval Constraints:** The train intervals are determined based on the selected number of trains per hour ($n_t$) using the formula $x_t = 60/n_t.$
5. **Relieve the direction of the train**: the same number of people board and alight on both the upward and downward subway routes.

---

## **3. Problem Definition**

### Final Passenger Load Calculation

The number of passengers at each train is calculated as:

$$
\text{Final Boarding per Train[1]} \cdot n_t = \text{Boarding[1]} - \text{Alighting[1]}
$$

$$
\text{Final Boarding per Train[i]} \cdot n_t = \text{Final Boarding per Train[i-1]} \cdot n_t + \text{Boarding[i]} - \text{Alighting[i]}\quad \text{if}\ i > 1
$$

### **Decision Variables**

- **Final Boarding per Train[i]:** Continuous variables representing the passenger load at each station per train.

### **Constants**

- train_capacity: 960
    - Represents the **maximum number of passengers** that a train can accommodate at one time.
- operating_cost_per_train: 10000
    - The **cost of operating a single train** for one hour, including expenses such as energy, crew wages, and maintenance.
- waiting_cost_per_minute: 10
    - The **cost incurred per minute** due to passengers waiting for a train. This cost is often used to quantify the negative impact of increased waiting times on passenger satisfaction.
- discomfort_constant: 10
    - A factor used to calculate the **discomfort cost** experienced by passengers, which is typically influenced by train crowding levels. A higher value represents greater sensitivity to crowding and discomfort.

### **Objective Function**

Minimize the total cost, consisting of:

$$
\text{Minimize: } C = C_{\text{operating}} + C_{\text{waiting}} + C_{\text{discomfort}}
$$

1. **Operating Cost:** The cost of running nt trains per hour:

$$
C_{\text{operating}} = n_t \cdot \text{operating cost per train}
$$

1. **Waiting Cost:** The cost of passengers waiting due to train intervals:

$$
C_{\text{waiting}} = x_t \cdot \text{waiting cost per minute} \cdot \text{Total Boarding Passengers}
$$

$$
\text{Total Boarding Passengers} = \sum_{i \in \text{Stations}}\text{Boarding Passengers[i]}
$$

1. **Discomfort Cost:** The cost of passenger discomfort:

$$
C_{\text{discomfort}} = \frac{\text{discomfort constant}}{\text{train capacity}} \cdot \sum_{i\in\text{Stations}} \text{Final Boarding per Train[i]} * n_k
$$

### **Constraints**

1. Passenger Flow Constraints: Ensure continuity of passenger flow between stations:
    
$$
\text{Final Boarding per Train[1]} \cdot n_t = \text{Boarding[1]} - \text{Alighting[1]}
$$
    
$$
\text{Final Boarding per Train[i]} \cdot n_t = \text{Final Boarding per Train[i-1]} \cdot n_t + \text{Boarding[i]} - \text{Alighting[i]}
$$
    
2. Capacity Constraints: Ensure train capacity meets passenger demand:
    
$$
\text{Final Boarding per Train[i]} \leq \text{train capacity}  \quad \forall i
$$
    

### **Interval Ranges**

The train intervals (x_t) are derived from the possible values of nt, calculated as:

$$
x_t = \frac{60}{n_t}
$$

## LP Model Results

|  | Weekdays (Average Time Interval) (min) | Weekends (Average Time Interval) (min) |
| --- | --- | --- |
| 03 ~ 04 | 38 | 38 |
| 04 ~ 05 | 37.9227053140 | 38 |
| 05 ~ 06 | 14.3091787440 | 17.3170731707 |
| 06 ~ 07 | 10.3478260870 | 13.5121951220 |
| 07 ~ 08 | 6.4057971014 | 11.4634146341 |
| 08 ~ 09 | 6.2898550725 | 8.3902439024 |
| 09 ~ 10 | 6.2125603865 | 8.1463414634 |
| 10 ~ 11 | 6.1932367150 | 7.3658536585 |
| 11 ~ 12 | 6.1739130435 | 6.8292682927 |
| 12 ~ 13 | 6.1352657005 | 6.3902439024 |
| 13 ~ 14 | 6.0966183575 | 6.1951219512 |
| 14 ~ 15 | 6.0966183575 | 6.1951219512 |
| 15 ~ 16 | 6.0772946860 | 6.1463414634 |
| 16 ~ 17 | 6.0772946860 | 6.0975609756 |
| 17 ~ 18 | 6.0772946860 | 6.0487804878 |
| 18 ~ 19 | 6.0966183575 | 6.4390243902 |
| 19 ~ 20 | 6.2318840580 | 8.5365853659 |
| 20 ~ 21 | 8.2222222222 | 9.1707317073 |
| 21 ~ 22  | 8.6859903382 | 9.6097560976 |
| 22 ~ 23 | 9.9613526570 | 10.0487804878 |
| 23 ~ 24 | 13.7874396135 | 14.3902439024 |
| 24 ~ 01 | 37.3623188406 | 37.4634146341 |
| 01 ~ 02 | 38 | 38 |
| 02 ~ 03 | 38 | 38 |
- 38 is the maximum time interval of our LP model. so it means that it can be more larger (even infinite)

## **Real World Example (compare with Daejeon subway time table, executed since 24.04.01)**

|  | Weekdays (Average Time Interval) (min) | Weekends (Average Time Interval) (min) |
| --- | --- | --- |
| 03 ~ 04 | - | - |
| 04 ~ 05 | - | - |
| 05 ~ 06 | 15 | 15 |
| 06 ~ 07 | 12 | 12 |
| 07 ~ 08 | 7.5 | 12 |
| 08 ~ 09 | 6 | 10 |
| 09 ~ 10 | 10 | 10 |
| 10 ~ 11 | 10 | 10 |
| 11 ~ 12 | 10 | 10 |
| 12 ~ 13 | 10 | 10 |
| 13 ~ 14 | 10 | 10 |
| 14 ~ 15 | 10 | 10 |
| 15 ~ 16 | 10 | 10 |
| 16 ~ 17 | 10 | 10 |
| 17 ~ 18 | 8.5714285714 | 7.5 |
| 18 ~ 19 | 6 | 8.5714285714 |
| 19 ~ 20 | 8.5714285714 | 10 |
| 20 ~ 21 | 10 | 10 |
| 21 ~ 22 | 10 | 12 |
| 22 ~ 23 | 12 | 12 |
| 23 ~ 24 | 15 | 15 |
| 24 ~ 01 | - | - |
| 01 ~ 02 | - | - |
| 02 ~ 03 | - | - |

## Dataset Reference

- Daejeon Subway current time table
    
    [https://www.djtc.kr/kor/stationInfo.do?menuIdx=38&tab=2](https://www.djtc.kr/kor/stationInfo.do?menuIdx=38&tab=2)
    
    [대전_전체 열차시간표.xlsx](Term%20Project%20(Eng)%20fc79daf68d9b4c89b592c16827b74a52/%25E1%2584%2583%25E1%2585%25A2%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB%25E1%2584%258E%25E1%2585%25A6_%25E1%2584%258B%25E1%2585%25A7%25E1%2586%25AF%25E1%2584%258E%25E1%2585%25A1%25E1%2584%2589%25E1%2585%25B5%25E1%2584%2580%25E1%2585%25A1%25E1%2586%25AB%25E1%2584%2591%25E1%2585%25AD.xlsx)
    
- Daejeon Number of boarding and alighting people by time
    
    [https://www.data.go.kr/data/15060591/fileData.do](https://www.data.go.kr/data/15060591/fileData.do)
    
    [대전교통공사_시간대별 승하차인원_20241031.csv](Term%20Project%20(Eng)%20fc79daf68d9b4c89b592c16827b74a52/%25E1%2584%2583%25E1%2585%25A2%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB%25E1%2584%2580%25E1%2585%25AD%25E1%2584%2590%25E1%2585%25A9%25E1%2586%25BC%25E1%2584%2580%25E1%2585%25A9%25E1%2586%25BC%25E1%2584%2589%25E1%2585%25A1_%25E1%2584%2589%25E1%2585%25B5%25E1%2584%2580%25E1%2585%25A1%25E1%2586%25AB%25E1%2584%2583%25E1%2585%25A2%25E1%2584%2587%25E1%2585%25A7%25E1%2586%25AF_%25E1%2584%2589%25E1%2585%25B3%25E1%2586%25BC%25E1%2584%2592%25E1%2585%25A1%25E1%2584%258E%25E1%2585%25A1%25E1%2584%258B%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%258B%25E1%2585%25AF%25E1%2586%25AB_20241031.csv)
