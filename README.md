# CSV FIlE HANDLING USING PYTHON:
---------------------------------
	- To know basics about file handling in python, kindly go through readme file in the below github link (section 5).

		- Github link -https://github.com/jagan-hazard/BASICS-IN-PYTHON-PROGRAMMING.

	CSV Module
	----------

	- csv module is object based form of reading and writing the csv file. It can read and write data in dictionary format as well as list format.
	- csv.DictReader will return the each row as dict with keys will be from the header and values for that keys is respective column value.
	- csv.writer will write the list into the csv file.
	- csv.DictWriter will write the dict into the csv file.

		- csv.reader(csvfile,dialect='excel', **fmtparams)   		- > for reading the data in list format
		- csv.writer(csvfile, dialect='excel', **fmtparams)   		- > for writing the data in list format
		- csv.DictReader(csvfile, fieldnames=None, restkey=None, restval=None, dialect='excel')  - > for reading the data in dict format
		- csv.DictWriter(csvfile, fieldnames=None, restkey=None, restval=None, dialect='excel')  - > for writing the data in dict format

	1.1. Methods in csv module:
	---------------------------
		1.1.1. csv.reader(csvfile,dialect='excel', **fmtparams)
		-------------------------------------------------------
		- This function will return an object having a list which contains the each row as list and each coloumn in that row as elements for the list.

		e.g:
			import csv
			with open('event.csv','r+', encoding="utf8") as file:
			    reader=csv.reader(file,)
			    for i in reader:	 # loop through each row
			        print(i)         # this will print whole row.
			      	k=0
	        		for j in i:		 # loop through each element in a row
	            		if (k==2):   # column number=2
	                		print(j) # print that column alone
	                	k+=1

		1.1.2. <reader>.__next__()
		---------------------------
		- This function will return a list which contains next row content in it. 
		- when used contionously, it will work like a iterator.

		e.g:
			import csv
			with open('event.csv','r+', encoding="utf8") as file:
			    reader=csv.reader(file,)
			    header=reader.__next__()      		# header row contents 
			    print(header)
			    row_1=reader.__next__()      		# first_row row contents 
			    print(row_1)

		1.1.3. <reader>.line_num
		---------------------------
		- This function will return a position of the row as per the pointer. 

		e.g:
			import csv
			with open('event.csv','r+', encoding="utf8") as file:
			    reader=csv.reader(file)
			    num=reader.line_num
			    print(num)                   # pointer at position zero
			    header=reader.__next__()     # first row is read
			    print(header)
			    num=reader.line_num
			    print(num)                  # pointer at position one

		1.1.4. csv.DictReader(csvfile,dialect='excel', **fmtparams)
		-------------------------------------------------------
		- This function will return a object having a ordered dictionary which contains the each column heading as keys and each coloumn content as values.
		- we need to use dict() to convert ordered dictionary into normal key and value pair dictionary.

		e.g:
			import csv
			with open('event.csv','r+', encoding="utf8") as file:
			    reader=csv.DictReader(file)
			    for i in reader:
			        print(dict(i))              # each row as a dictionary
			        print(i['Contact Email'])   # directly print the mentioned column


		1.1.5. csv.writer(csvfile,dialect='excel', **fmtparams)
		-------------------------------------------------------
			- This function will write the given content in list format into the csv file.
			- In windows, sometimes after each write operation it will add empty line, to avoid this issue, we need to add (newline='') while openning the file.
			- Input data must be in list format.

			e.g-1:  write and then read the same file
			--------------------------------------------
				import csv
				a=['Start Date ', 'Start Time', 'End Date', 'End Time', 'Event Title ', 'All Day Event', 'No End Time', 'Event Description', 'Contact ', 'Contact Email', 'Contact Phone', 'Location', 'Category', 'Mandatory', 'Registration', 'Maximum', 'Last Date To Register']
				b=['9/5/2011', '3:00:00 PM', '9/5/2011', '', 'Social Studies Dept. Meeting', 'N', 'Y', 'Department meeting', 'Chris Gallagher', 'cgallagher@schoolwires.com', '814-555-5179', 'High School', '2', 'N', 'N', '250', '9/2/2011']
				c=['9/5/2011', '3:00:00 PM', '9/5/2011', '', 'Social Studies Dept. Meeting', 'N', 'Y', 'Department meeting', 'Chris Gallagher', 'cgallagher@schoolwires.com', '814-555-5179', 'High School', '2', 'N', 'N', '250', '9/2/2011']
				d=['9/5/2011', '3:00:00 PM', '9/5/2011', '', 'Social Studies Dept. Meeting', 'N', 'Y', 'Department meeting', 'Chris Gallagher', 'cgallagher@schoolwires.com', '814-555-5179', 'High School', '2', 'N', 'N', '250', '9/2/2011']
				
				with open('event1.csv','r+',newline='', encoding="utf8") as file:       # newline=''   -> this will avoid 																					unwanted empty line insertion between 																		each writerow operation.
				    writer=csv.writer(file, delimiter=',', quotechar='"', quoting=csv.QUOTE_MINIMAL)
				    writer.writerow(a)                  # write each row
				    writer.writerow(b)                  # write each row
				    writer.writerow(c)                  # write each row
				    writer.writerow(d)                  # write each row
				with open('event1.csv','r+', encoding="utf8") as file:
				    reader=csv.reader(file)             # reader
				    for i in reader:
				        print(i)                        # print each row

			e.g-2:  fetch data from one csv file and parallely write into another file
			-----------------------------------------------------------------------------	

				import csv  
				with open('event.csv','r+', encoding="utf8") as file1:        			# file1 for reading
				    with open('event2.csv','w+',newline='', encoding="utf8") as file2:	# file2 for writing
				        reader=csv.reader(file1)   
				        writer=csv.writer(file2, delimiter=',', quotechar='"', quoting=csv.QUOTE_MINIMAL)
				        for i in reader:             # read file 1
				            writer.writerow(i)       # write the content into the file parallely.   	

			e.g-3  fetch specific column datas from one csv file and parallely write into another file using writer()
			----------------------------------------------------------------------------------------------------------

				import csv  
				with open('event.csv','r+', encoding="utf8") as file1: 
				    with open('event3.csv','w+', encoding="utf8") as file2:
				        reader=csv.DictReader(file1)   
				        writer=csv.writer(file2)
				        writer.writerow(['Start Time','Contact ','Contact Email'])
				        for i in reader:             													# read file 1
				            a=dict(i)
				            if all(key in a for key in ['Start Time','Contact ','Contact Email']):   #this loop will check 																respective column as per heading values.
				                writer.writerow([a['Start Time'],a['Contact '],a['Contact Email']])       # write the content 																		into the file parallely.    
				                print ([a['Start Time'],a['Contact '],a['Contact Email']])

		1.1.6. writerow(row)
		---------------------
			- This function will write the given list/dictionary(as per writer function) as a single row into the file.
			- Irrespective of data size, this function will write it in single row.
			- Input data must be in list/dictionary format.
			- syntax: <writer>.writerow(row)

		1.1.7. writerows(rows)
		---------------------
			- This function will write the given list/dictionary(as per writer function) in a iterable object, into multile rows in the file.
			- Input data must be in list/dictionary format in a iterable object.
			- syntax: <writer>.writerows(rows)

			e.g: 
			----
				import csv  
				with open('event.csv','r+', encoding="utf8") as file1: 
				    with open('event3.csv','w+',newline='', encoding="utf8") as file2:
				        reader=csv.reader(file1)   
				        writer=csv.writer(file2)  # remember writer is an object
				        writer.writerows(reader)              # write all the contents row by row.(in a single go)

		1.1.8. writeheader()  -> (used for DictWriter() function alone)
		---------------------------------------------------------------
			- This function will write header row for the file.
			- Input data must be in list/dictionary format in a iterable object.
			- syntax: <writer>.writerows()
			- write header is used for DictWriter(), because it won't have first row which(header row) is distributed as keys for all the rows.

			e.g: 
			----
				import csv  
				with open('event.csv','r+', encoding="utf8") as file1: 
				    with open('event3.csv','w+',newline='', encoding="utf8") as file2:
				        reader=csv.reader(file1)   
				        writer=csv.writer(file2)  # remember writer is an object
				        writer.writerows(reader)              # write all the contents row by row.(in a single go)

		1.1.9. csv.Dictwriter(csvfile,fieldnames,dialect='excel', **fmtparams)
		------------------------------------------------------------------------
			- This function will the write given input in dictionary format into the file.
			- the input data must be in dictionary format.
			- fieldname parameter is must for DictWriter, since it will tell the writer which order of the column will written

			e.g: fetch specific column datas from one csv file and parallely write into another file using DictWriter()
			-----------------------------------------------------------------------------------------------------------
		        import csv  
				with open('event.csv','r+', encoding="utf8") as file1: 
				    with open('event3.csv','w+', encoding="utf8") as file2:
				        reader=csv.DictReader(file1)
				        fieldnames=['Start Time','Contact ','Contact Email']			#specifying the column name
				        writer=csv.DictWriter(file2,fieldnames=fieldnames)
				        writer.writeheader()     # write the header row
				        for i in reader:
				            a=dict(i)           # convert from ordered_dic to dict
				            writer.writerow({'Start Time':a['Start Time'],'Contact ':a['Contact '],'Contact Email':a['Contact Email']})
				  
