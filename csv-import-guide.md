  Fintify CSV Import Guide                                                      
                                                                                
  Fintify supports CSV import so you can bring your investment records into the
  app — whether you're migrating from another tool, restoring from a backup, or 
  managing transactions in a spreadsheet.                   
                                                                                
  ---                                                       
  The Easiest Way to Start
                                                                                
  The fastest way to get a correctly formatted file is to export your data from 
  Fintify first, then use that file as your template.                           
                                                            
  Go to Settings → Export CSV, open the file in a spreadsheet app, and add or   
  edit rows in the same format. When you're done, import the updated file back
  into Fintify.                                                                 
                                                            
  If you're starting fresh with no existing data in Fintify, follow the format  
  described below.
                                                                                
  ---                                                       
  File Format Overview
                                                                                
  Your CSV file must include a header row. Fintify uses a single unified format
  for all activity types — investments and cash transfers share the same        
  structure, with different columns filled in depending on the row type.
                                                                                
  Required columns (all CSV files must have these column headers):              
   
  ┌───────────────────┬──────────────────────────────────────────────────────┐  
  │      Column       │                     Description                      │
  ├───────────────────┼──────────────────────────────────────────────────────┤
  │ date              │ Activity date. Use YYYY-MM-DD format when possible.  │
  ├───────────────────┼──────────────────────────────────────────────────────┤
  │ activity_category │ Use transaction for buy/sell rows. Use transfer for  │  
  │                   │ deposit/withdraw rows.                               │
  ├───────────────────┼──────────────────────────────────────────────────────┤  
  │ type              │ Use buy, sell, deposit, or withdraw.                 │
  ├───────────────────┼──────────────────────────────────────────────────────┤  
  │ account_name      │ Your account label, such as TFSA, RRSP, or any name  │
  │                   │ you use.                                             │  
  ├───────────────────┼──────────────────────────────────────────────────────┤
  │ owner_name        │ Owner label, such as your initials or name. Used     │  
  │                   │ when tracking multiple people.                       │  
  ├───────────────────┼──────────────────────────────────────────────────────┤
  │ account_type      │ Account type, such as TFSA, RRSP, Cash, Margin, or   │  
  │                   │ A股账户.                                             │  
  ├───────────────────┼──────────────────────────────────────────────────────┤
  │ institution       │ Your brokerage or institution, such as Questrade,    │  
  │                   │ Wealthsimple, RBC, or 国金证券.                      │  
  ├───────────────────┼──────────────────────────────────────────────────────┤
  │ currency          │ Use USD, CAD, or CNY.                                │  
  └───────────────────┴──────────────────────────────────────────────────────┘

  Additional columns for buy/sell rows:                                         
   
  ┌───────────────┬─────────────┬────────────────────────────────────────────┐  
  │    Column     │  Required   │                Description                 │
  ├───────────────┼─────────────┼────────────────────────────────────────────┤
  │ ticker        │ Yes         │ The security ticker, such as AAPL, ENB.TO, │
  │               │             │  or 600036.                                │
  ├───────────────┼─────────────┼────────────────────────────────────────────┤  
  │ quantity      │ Yes         │ Number of shares. Use a positive number    │
  │               │             │ for both buy and sell.                     │  
  ├───────────────┼─────────────┼────────────────────────────────────────────┤
  │ price         │ Yes         │ Price per share.                           │  
  ├───────────────┼─────────────┼────────────────────────────────────────────┤
  │ api_symbol    │ Recommended │ Market data symbol. Needed for China       │
  │               │             │ A-shares — use 600036.SS or 000651.SZ.     │  
  ├───────────────┼─────────────┼────────────────────────────────────────────┤
  │ market        │ Optional    │ Use us, canada, or chinaA to explicitly    │  
  │               │             │ set the market.                            │  
  ├───────────────┼─────────────┼────────────────────────────────────────────┤
  │ security_name │ Optional    │ Security name, such as Apple Inc or        │  
  │               │             │ 招商银行.                                  │  
  ├───────────────┼─────────────┼────────────────────────────────────────────┤
  │ fee           │ Optional    │ Transaction fee or commission.             │  
  ├───────────────┼─────────────┼────────────────────────────────────────────┤
  │ notes         │ Optional    │ Any notes you want to keep.                │
  └───────────────┴─────────────┴────────────────────────────────────────────┘  
   
  Additional columns for deposit/withdraw rows:                                 
                                                            
  ┌─────────────┬──────────┬─────────────────────────────────────────────────┐
  │   Column    │ Required │                   Description                   │
  ├─────────────┼──────────┼─────────────────────────────────────────────────┤
  │ cash_amount │ Yes      │ The cash amount. Use a positive number for both │
  │             │          │  deposits and withdrawals.                      │
  ├─────────────┼──────────┼─────────────────────────────────────────────────┤  
  │ notes       │ Optional │ Any notes.                                      │
  └─────────────┴──────────┴─────────────────────────────────────────────────┘  
                                                            
  ---
  Ticker Format and Market Detection
                                                                                
  Fintify determines the market automatically from your ticker:
                                                                                
  ┌───────────────────────┬──────────────────────────┬──────────────────────┐   
  │        Market         │      Ticker format       │       Examples       │
  ├───────────────────────┼──────────────────────────┼──────────────────────┤   
  │ US stocks and ETFs    │ Plain ticker             │ AAPL, MSFT, NVDA,    │
  │                       │                          │ VGT                  │
  ├───────────────────────┼──────────────────────────┼──────────────────────┤   
  │ Canadian stocks and   │ Ticker with .TO, .V, or  │ ENB.TO, BNS.TO,      │   
  │ ETFs                  │ .NE                      │ VDY.TO               │   
  ├───────────────────────┼──────────────────────────┼──────────────────────┤   
  │ China A-shares and    │ Plain ticker (no suffix) │ 600036, 000651,      │
  │ ETFs                  │                          │ 510300               │   
  └───────────────────────┴──────────────────────────┴──────────────────────┘
                                                                                
  For China A-shares, also fill in the api_symbol column with the full provider 
  symbol: 600036.SS, 000651.SZ, etc. Shanghai-listed stocks use .SS,
  Shenzhen-listed use .SZ.                                                      
                                                            
  If you want to explicitly set the market instead of relying on automatic      
  detection, use the market column with values us, canada, or chinaA.
                                                                                
  ---                                                       
  Example CSV

  source_record_id,date,activity_category,type,account_name,owner_name,account_t
  ype,institution,ticker,api_symbol,security_name,market,currency,quantity,price
  ,cash_amount,fee,notes
  dep-2024-01-15,2024-01-15,transfer,deposit,WD-TFSA-Questrade,WD,TFSA,Questrade
  ,,,,,,,,5000,,Initial deposit                                                 
  buy-2024-01-16,2024-01-16,transaction,buy,WD-TFSA-Questrade,WD,TFSA,Questrade,
  AAPL,AAPL,Apple Inc,us,USD,10,185.50,,1.00,Initial position                   
  buy-2024-02-10,2024-02-10,transaction,buy,WD-TFSA-Questrade,WD,TFSA,Questrade,
  ENB.TO,ENB.TO,Enbridge Inc,canada,CAD,50,48.25,,0.00,Canadian dividend stock  
  buy-2024-03-05,2024-03-05,transaction,buy,WD-RRSP-Questrade,WD,RRSP,Questrade,
  NVDA,NVDA,NVIDIA Corp,us,USD,5,850.00,,1.00,Before split                      
  buy-2024-03-10,2024-03-10,transaction,buy,WD-China-Guojin,WD,A股账户,国金证券,
  600036,600036.SS,招商银行,chinaA,CNY,1000,38.20,,5.00,A-share position        
  sell-2024-06-20,2024-06-20,transaction,sell,WD-TFSA-Questrade,WD,TFSA,Questrad
  e,AAPL,AAPL,Apple Inc,us,USD,2,210.00,,1.00,Partial sale                      
  wdr-2024-07-01,2024-07-01,transfer,withdraw,WD-TFSA-Questrade,WD,TFSA,Questrad
  e,,,,,,,,1000,,Cash withdrawal                                                
                                                            
  The source_record_id column is optional but recommended if you plan to import 
  the same file multiple times — it prevents duplicate records.
                                                                                
  ---                                                       
  Starting Without Complete History
                                                                                
  You don't need every historical trade to start using Fintify.
                                                                                
  If you already hold a position but don't have the full transaction history,   
  add one initial buy row using your current share count and average cost per   
  share:                                                                        
                                                            
  date,activity_category,type,account_name,owner_name,account_type,institution,t
  icker,api_symbol,market,currency,quantity,price                               
  2026-04-28,transaction,buy,WD-RRSP-Questrade,WD,RRSP,Questrade,MSFT,MSFT,us,US
  D,50,320.00                                                                   
                                                            
  This lets Fintify track your position going forward. Gain/loss and historical 
  performance before this date will not reflect your actual history.
                                                                                
  If you don't know your exact average cost, you can use an estimate or current 
  market price. Keep in mind that gain/loss figures will be based on this
  estimated cost.                                                               
                                                            
  ---
  Import Tips
             
  - Use one row per activity.
  - Keep account_name, owner_name, account_type, and institution consistent for 
  the same account across all rows. Fintify uses these fields together to       
  identify an account.                                                          
  - Use YYYY-MM-DD date format when possible. Other formats such as MM/DD/YYYY  
  are also accepted.                                                            
  - Use positive numbers for both buy and sell quantities, and for both deposit
  and withdraw amounts.                                                         
  - Export a backup from Fintify before importing large changes.
  - Review your portfolio after importing to confirm holdings and values look   
  correct.                                                                      
                                                                                
  ---                                                                           
  Data Accuracy                                             
               
  Fintify relies on the data you import. Incomplete, duplicated, or inconsistent
   records may result in inaccurate portfolio values, cost basis, gain/loss     
  figures, or dividend summaries.
                                                                                
  Market data, dividends, exchange rates, and corporate actions are sourced from
   third-party providers and may be delayed, incomplete, or unavailable. Verify
  important financial figures with your brokerage, financial advisor, or tax    
  professional.                                             

  ---
  Privacy
         
  CSV files may contain investment records and financial information. Store and
  share your exported files carefully.                                          
   
  Fintify is a local-first app. Your data stays on your device unless you choose
   to export or share it.                                   
                                                                                
  ---                                                       
  Need Help?
            
  If you run into issues or have questions, contact us at fintify.support@gmail.com .
                                                                                
  When reporting an import issue, it helps to include your app version, iOS     
  version, a description of what went wrong, and a sample of the rows that      
  failed (with any sensitive information removed). 
