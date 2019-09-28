# sapaap
**S**AP **A**bap **A**udit logs **P**arser and converter

This is a simple console tool for parsing SAP abap audit logs and converting them into readable CSV format.

## Usage
Just read --help
```
Use sapaap [options] <filename>
or in pipe <output> | sapaap [options]
options:
  -BE
    	set if audit file from BigEndian system (AIX, HP-UX, Solaris, etc)
  -NUC
    	set if SAP system is NON-UNICODE
  -a string
    	string to append to every row in result data
    	example: "$HOST,$SAPSYSTEM" 
  -d string
    	delimiter to separate values in output records (CSV) (default ",")
  -describe
    	get audit file format description
```
## Output format
```
 Audit file format:                                                             
 Pos   Size    Field name      Description                                      
 ----------------------------------------                                       
 1     [1]     Version         file format version (I guess)                    
 2     [3]     MessageID       audit message identificator                      
 5     [4]     Year                                                             
 9     [2]     Month                                                            
 11    [2]     Day                                                              
 13    [2]     Hour                                                             
 15    [2]     Minute                                                           
 17    [2]     Second                                                           
 19    [7]     OS_PID          OS Process ID                                    
 26    [5]     SAP_PID         SAP Process ID                                   
 31    [1]     LogonType       Type of connection (Dialog, RFC, etc)            
 32    [1]     SAP_PID_hex     SAP Process ID in hex (I guess)                  
 33    [8]     Unknown         I couldn't detect what that field means          
 41    [12]    UserName        User LOGIN                                       
 53    [20]    Transaction                                                      
 73    [40]    Report                                                           
 113   [3]     Mandant                                                          
 116   [1]     SessionID                                                        
 117   [64]    Parameters      most of messages has parameters, so here they are
 181   [20]    Terminal        host name user's PC                              
																				
 The record length is 200 char symbols.                                         
 And 200 bytes in NUC system.		                                            
 In case of UINCODE system - 400 bytes because of 2 bytes per 1 symbol          
																				
    																			
 Output CSV format:                                                             
 Pos   Size    Field name      Description                                      
 ----------------------------------------                                       
 1     [10]    Date            YYYY-MM-DD ISO8601                               
 2     [19]    Timestamp       YYYY-MM-DD HH:MM:ss                              
 3     [3]     MessageID                                                        
 4     [1]     LogonType                                                        
 5     [0-12]  UserName                                                         
 6     [0-20]  Transaction                                                      
 7     [7]     OS_PID          OS Process ID                                    
 8     [5]     SAP_PID         SAP Process ID                                   
 9     [0-40]  Report                                                           
 10    [3]     Mandant                                                          
 11    [1]     SessionID                                                        
 12    [0-64]  Parameters      most of messages has parameters, so here they are
 13    [0-20]  Terminal        host name user's PC                              
 14... [...]   <append>        custom appended string                           
```
## Build and use
Try `go get` / `go install`

For linux try run:

	$ go get github.com/laoleesch/sapaap
	$ GOOS=linux GOARCH=amd64 go build -o sapaap

## License
