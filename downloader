from __future__ import print_function
from googleapiclient.discovery import build
from httplib2 import Http
from oauth2client import file, client, tools
from googleapiclient.http import MediaFileUpload,MediaIoBaseDownload
import os,io

SCOPES = 'https://www.googleapis.com/auth/drive'

t = os.path.realpath('token.json')
store = file.Storage(t)
creds = store.get()
if not creds or creds.invalid:
    k = os.path.realpath('credentials.json')
    flow = client.flow_from_clientsecrets(k, SCOPES)
    creds = tools.run_flow(flow, store)
service = build('drive', 'v3', http=creds.authorize(Http()))

name_list = []
ids=[]


def list_of_files():

    """Shows basic usage of the Drive v3 API.
    Prints the names and ids of the first 10 files the user has access to.
    """

    # Call the Drive v3 API
    results = service.files().list(pageSize=10, fields="nextPageToken, files(id, name)").execute()
    items = results.get('files', [])

    if not items:
        print('No files found.')
    else:
        print('Files:')
        count=int(0)
        #name_list = []
        for item in items:
            #print(u'{0} ({1})'.format(item['name'], item['id']))
            name=item['name']
            name_list.append(name)
            id=item['id']
            ids.append(id)
            print(str(count)+':\t'+str(name)+'\t'+str(id))
            count+=1



def Download_drive():
    num=input("1.enter the list number ex:(1) 2. file name ex(unmaed.jpg) 3. id ex(hafvhsdvsdvisdfnvidsb4654sdafsd)")
    if '1'==num:
        count_no=input("enter the count_no")
        temp=ids[int(count_no)]
        name=name_list[int(count_no)]
        download_types(temp,name)
    elif '2' == num:
        name=input("enter the name")
        temp=name_list.index(name)
        id=ids[int(temp)]
        download_types(id,name)
    elif '3' == num:
        id = input("enter the id")
        temp=ids.index((id))
        name=name_list[int(temp)]
        download_types(id,name)





def download_types(temp,name):
    request = service.files().get_media(fileId=temp)
    fh = io.BytesIO()
    downloader = MediaIoBaseDownload(fh, request)
    done = False
    while done is False:
        status, done = downloader.next_chunk()
        print("Download %d%%." % int(status.progress() * 100))
    with io.open(name,'wb') as f:
        fh.seek(0)
        f.write(fh.read())
if __name__ == '__main__':
    list_of_files()
    Download_drive()
