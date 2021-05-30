```python
# Make the data that it could list the sequence of the name list to handle 

seq_df = pd.DataFrame( columns = [ 'timestamp','people','avble_date','avble_count','date_with_max_pro','max_pro_value_for_date','overall_supply_rate'] )

for i in range( 1, max_avb_count+1):
    
    # Start from choosing people with min available date
    target_df = people_df[         people_df['avble_count'] ==  i   ].reset_index(drop=True)
    
    
    if( len(target_df) != 0  ):
        
        print( '\nAvailable date count is {}'.format(i) )
        
        target_df['date_with_max_pro'] = 0 # set date 0 as default
        target_df['max_pro_value_for_date'] = 0 # set date 0 as default
        
        
        
        # Calculate the probability of each person
        for j in range( len(target_df) ):
            
            overall_supply_rate = 0
            avble_list = target_df.loc[j,'avble_date']
            
            
            print("The person is {}".format (target_df.loc[j,'people']) )
            
            for k in range( len(avble_list) ):
                
                # k is for list element, i is for division
                target_time_df = time_df[  time_df['date'] == avble_list[k]  ].reset_index(drop=True)
                
                supply_rate = ( target_time_df.loc[0,'supply_count'] / target_time_df.loc[0,'demand_count'] )
                overall_supply_rate = overall_supply_rate + (1/i) * supply_rate
                
                if( supply_rate > target_df.loc[j,'max_pro_value_for_date']  ):
                    
                    target_df.loc[j,'date_with_max_pro'] = avble_list[k]
                    target_df.loc[j,'max_pro_value_for_date'] = supply_rate
                    
                
                
            print( "The probability is {}\n".format(overall_supply_rate) )
            target_df.loc[j,'overall_supply_rate'] = overall_supply_rate
        

        target_df = target_df.sort_values("overall_supply_rate", ascending=True).reset_index(drop=True)

#         print(target_df)
        
        seq_df = pd.concat( [ seq_df,target_df] )
        

            
seq_df


```
