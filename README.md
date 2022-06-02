# .db-to-.csv-conversion
Converting a .db file to .csv to DataFrame using Python and SQLite3

### Create filepath

	  dbfile = '/Users/dmsta/OneDrive/Desktop/ITAI2377/MISC/chat.db'

### Establish a connection and create cursor with sqlite3.connect
- https://pythonprogramming.net/sql-database-python-part-1-inserting-database/

		conn = sqlite3.connect(dbfile)
		cur = conn.cursor()

### Read from database
- https://pythonprogramming.net/sqlite-part-3-reading-database-python/?completed=/sqlite-part-2-dynamically-inserting-database-timestamps/


		def read_from_db():
			cur.execute('SELECT * FROM chat')
			data = cur.fetchall()
			print(data)
			for row in data:
				print(row)

### Read in pandas dataframe by finding table name in database
- https://stackoverflow.com/questions/62340498/open-database-files-db-using-python

		table_list = [a for a in cur.execute("SELECT name FROM sqlite_master WHERE type = 'table'")]

		print(table_list)

### Read from selected table name
- Table names selection:
    - _SqliteDatabaseProperties
    - deleted_messages
    - sqlite_sequence
    - chat_handle_join
    - sync_deleted_messages
    - message_processing_task
    - handle
    - sync_deleted_chats
    - message_attachment_join
    - sync_deleted_attachments
    - kvtable
    - chat_message_join
    - ### message
    - chat
    - attachment
    - sqlite_stat1

    		def read_from_table_list():
				cur.execute('SELECT * FROM message')
				data = cur.fetchall()
				print(data)
				for row in data:
					print(row)

			header = []
			cur = conn.cursor()
			for column in cur.execute('PRAGMA table_info("message")'):
				header.append(column[1])
				
### Convert table to .csv file
		sql_query = pd.read_sql_query('''SELECT * FROM message''',conn)
		df = pd.DataFrame(sql_query)
		df.to_csv (r"C:\Users\dmsta\OneDrive\Desktop\ITAI2377\MISC\message.csv")
		
### Read .csv file and create/copy DataFrame
		df = pd.read_csv('message.csv')
		msg = df.copy()
