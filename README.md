ftpretty
========

A wrapper for simple FTP operations: get, put, list etc.

The goal of this library is to provide a frictionless experience to FTPing files
in way similar to Amazon's s3cmd command line tool. The API should be intuitive
with the order of arguments reflecting the transfer direction of bytes.

Transfers are assumed to be binary.

Unrecognized commands fall through to the underlying FTP or FTP_TLS connection object

Examples:

    from ftpretty import ftpretty

    # kwargs are passed to underlying FTP or FTP_TLS connection
    # secure argument switches to an FTP_TLS connection
    f = ftpretty(host, user, pass, secure=False, timeout=10)

    # Get a file, save it locally
    f.get('someremote/file/on/server.txt', '/tmp/localcopy/server.txt')

    or 

    # Get a file and write to an open file
    myfile = open('/tmp/localcopy/server.txt', 'wb')
    f.get('someremote/file/on/server.txt', myfile)

    or

    # Get a file and return contents
    contents = f.get('someremote/file/on/server.txt')

    # Put a local file to a remote location
    # non-existent subdirectories will be created automatically
    f.put('/tmp/localcopy/data.txt', 'someremote/file/on/server.txt')

    or

    # Put using an open file desciptor
    myfile = open('/tmp/localcopy/data.txt', 'r')
    f.put(myfile,  'someremote/file/on/server.txt')

    or

    # Put using string data
    f.put(None,  'someremote/file/greeting.txt', contents='blah blah blah')

    # Return a list the files in a directory
    f.list('someremote/folder')
    ['a.txt', 'b.txt']

    f.list('someremote/folder', extra=True)
    [{'date': 'Feb 11',
      'directory': 'd',
      'group': '1006',
      'items': '3',
      'name': 'a.txt',
      'owner': '1005',
      'perms': 'rwxr-xr-x',
      'size': '4096',
      'time': '02:03',
      'year': '2014'},
     {'date': 'Feb 11',
      'directory': 'd',
      'group': '1006',
      'items': '3',
      'name': 'b.txt',
      'owner': '1005',
      'perms': 'rwxr-xr-x',
      'size': '4096',
      'time': '02:35',
      'year': '2014'}]
    
    # Change to remote directory
    f.cd('someremote/folder')

    # Delete a remote file 
    f.delete('someremote/folder/file.txt')

    # Close the connection
    f.close()

