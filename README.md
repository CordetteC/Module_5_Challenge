This Module was completed with the help of Xpert and Instructor office hours:

Instructor Guidance:
#Our data should be uniquely identified by Mouse ID and Timepoint
duplicate_ID=combined_df.loc[combined_df.duplicated(subset=['Mouse ID', 'Timepoint']),'Mouse ID'].unique() 

#Create a clean DataFrame by dropping the duplicate mouse by its ID.
clean_combined_df= combined_df[combined_df['Mouse ID'].isin(duplicate_ID)==False]
clean_combined_df.head()

XPERT Learning Assistant:
#Group the DataFrame by 'Drug Regimen' and calculate summary statistics for 'Tumor Volume (mm3)'
summary_stats = clean_combined_df.groupby('Drug Regimen')['Tumor Volume (mm3)'].agg(
    mean='mean',                    # Calculate the mean of tumor volume
    median='median',                # Calculate the median of tumor volume
    variance='var',                 # Calculate the variance of tumor volume
    standard_deviation='std',       # Calculate the standard deviation of tumor volume
    SEM=st.sem                      # Calculate the Standard Error of the Mean using scipy's sem function
).reset_index()                     # Reset the index to convert the grouped column back into a regular column

#Set the 'Drug Regimen' as the index of the summary DataFrame
summary_stats.set_index('Drug Regimen', inplace=True)

median = clean_combined_df.groupby('Drug Regimen').median()['Tumor Volume (mm3)']
variance = clean_combined_df.groupby('Drug Regimen').var()['Tumor Volume (mm3)']
sd = clean_combined_df.groupby('Drug Regimen').std()['Tumor Volume (mm3)']
sem = clean_combined_df.groupby('Drug Regimen').sem()['Tumor Volume (mm3)']

Capomulin_average = Capomulin_data.groupby(['Mouse ID'])[['Tumor Volume (mm3)', 'Weight (g)']].mean().reset_index()
