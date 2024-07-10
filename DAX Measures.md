# [DAX] Hotel Revenue Insights and Analysis 


#### Revenue 

         = SUM(fact_bookings[revenue_realized])

#### Total Bookings
       
         = COUNT(fact_bookings[booking_id])  
         
#### Total Capacity

         = SUM(fact_aggregated_bookings[capacity])

#### Total Successful Bookings 

         = SUM(fact_aggregated_bookings[successful_bookings])
     
#### Occupancy %

        = DIVIDE(
            [Total Successful Bookings],
            [Total Capacity],
            0 )

#### Average Rating

        = AVERAGE(fact_bookings[ratings_given])

#### No of Days

        = DATEDIFF(
            MIN(dim_date[date]),  
            MAX(dim_date[date]),
            DAY)
          + 1

#### Total Cancelled Bookings

        = CALCULATE(
            [Total Bookings],
            fact_bookings[booking_status] = "Cancelled")

#### Cancellation %

        = DIVIDE(
            [Total Cancelled Booking],
            [Total Bookings])

#### Total Checked Out

        = CALCULATE(
            [Total Bookings],
            fact_bookings[booking_status] = "Checked Out")

#### Total No Show Bookings

        = CALCULATE(
            [Total Bookings],
            fact_bookings[booking_status] = "No Show")

#### No Show Rate %

        = DIVIDE(
            [Total No Show Bookings],
            [Total Bookings])

#### Booking % by Platform

        = DIVIDE(
            [Total Bookings],
            CALCULATE(
              [Total Bookings],
              ALL(
                fact_bookings[booking_platform])))

#### Booking % by Room class

        = DIVIDE(
            [Total Bookings],
            CALCULATE(
              [Total Bookings],
              ALL(
                dim_rooms[room_class])))

#### ADR (Average Daily Rate)

        = DIVIDE(
            [Revenue],
            [Total Bookings],
            0)

#### Realisation % (successful "checked out" percentage over all bookings )

        = 1 - ([Cancellation %] + [No Show Rate %])

#### RevPAR (Revenue Per Available Room)

        = DIVIDE(
            [Revenue],
            [Total Capacity])
            
#### DBRN (Daily Booked Room Nights)

        = DIVIDE(
            [Total Bookings],
            [No of Days])

#### DSRN (Daily Sellable Room Nights)

        = DIVIDE(
          [Total Capacity],
          [No of Days])

#### DURN (Daily Utilised Room Nights)

        = DIVIDE(
            [Total Checked Out],
            [No of Days])

#### Revenue WoW Change %

        = 
        VAR selw =
        IF(                        
          HASONEFILTER(dim_date[wn]),
          SELECTEDVALUE(dim_date[wn]),
          MAX(dim_date[wn]) )
          
        VAR revcw =
        CALCULATE(
          [Revenue],
          dim_date[wn] = selw)

        VAR revpw =
          CALCULATE(
            [Revenue],
            FILTER(                
              ALL(dim_date),
              dim_date[wn] = selw - 1))
              
        RETURN 
        DIVIDE( revcw, revpw, 0 )

#### Occupancy WoW Change %

        = 
        VAR selw =
        IF(
          HASONEFILTER(dim_date[wn]),
          SELECTEDVALUE(dim_date[wn]),
          MAX(dim_date[wn]) )

        VAR revcw =
          CALCULATE(
            [Occupancy %],
            dim_date[wn] = selw)

        VAR revpw =
        CALCULATE(
          [Occupancy %],
          FILTER(
            ALL(dim_date),
            dim_date[wn] = selw - 1) )

        RETURN
        DIVIDE( revcw, revpw, 0 )

#### ADR WoW Change %

        = 
        VAR selw =
        IF(
          HASONEFILTER(dim_date[wn]),
          SELECTEDVALUE(dim_date[wn]),
          MAX(dim_date[wn]) )

        VAR revcw =
          CALCULATE(
            [ADR],
            dim_date[wn] = selw)

        VAR revpw =
        CALCULATE(
          [ADR],
          FILTER(
            ALL(dim_date),
            dim_date[wn] = selw - 1) )

        RETURN
        DIVIDE( revcw, revpw, 0 )

#### RevPAR WoW Change %

        = 
        VAR selw =
        IF(
          HASONEFILTER(dim_date[wn]),
          SELECTEDVALUE(dim_date[wn]),
          MAX(dim_date[wn]) )

      VAR revcw =
        CALCULATE(
          [RevPAR],
          dim_date[wn] = selw)

      VAR revpw =
      CALCULATE(
        [RevPAR],
        FILTER(
          ALL(dim_date),
          dim_date[wn] = selw - 1) )

      RETURN
      DIVIDE( revcw, revpw, 0 )

#### Realisation WoW Change %

        = 
        VAR selw =
        IF(
          HASONEFILTER(dim_date[wn]),
          SELECTEDVALUE(dim_date[wn]),
          MAX(dim_date[wn]) )

        VAR revcw =
          CALCULATE(
            [Realisation %],
            dim_date[wn] = selw)

        VAR revpw =
        CALCULATE(
          [Realisation %],
          FILTER(
            ALL(dim_date),
            dim_date[wn] = selw - 1) )

        RETURN
        DIVIDE( revcw, revpw, 0 )

#### DSRN WoW Change %

        = 
        VAR selw =
        IF(
          HASONEFILTER(dim_date[wn]),
          SELECTEDVALUE(dim_date[wn]),
          MAX(dim_date[wn]) )

        VAR revcw =
          CALCULATE(
            [DSRN],
            dim_date[wn] = selw)

        VAR revpw =
        CALCULATE(
          [DSRN],
          FILTER(
            ALL(dim_date),
            dim_date[wn] = selw - 1) )

        RETURN
        DIVIDE( revcw, revpw, 0 )  
