# The 'all' rule depends on the 'bye.txt' target.                                         
all: bye.txt                                                                              
	# "all done!"
																					   
# The 'bye.txt' rule depends on the 'hello.txt' target.  It will run                      
# the hello.txt rule first if hello.txt is missing or out of date, then                   
# run the command to create bye.txt.                                                      
bye.txt: hello.txt                                                                        
	# exit 1
	echo "Byebye!" > bye.txt                                                              
																				   
# The 'hello.txt' rule makes the file 'hello.txt'.                                        
hello.txt:                                                                                
	echo "Hello, World!" > hello.txt             
