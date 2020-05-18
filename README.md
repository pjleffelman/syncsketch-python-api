# Syncsketch-API
This package provides methods to communicate with the syncsketch servers and wraps CRUD (create, reade, update, delete) methods to interact with Reviews.

### Getting Started



#### Installation

    pip install git+https://github.com/syncsketch/python-api.git


#### Compatibility
This library was tested with and confirmed on python versions:
- 2.7.14+
- 3.6
- 3.8

### Basic Examples


#### Authentication
Before we can start working, we need to get an `API_KEY` which you can obtain from the syncsketch [settings page](https://syncsketch.com/pro/#userProfile/settingsTab). Follow the given link, login and scroll down to the bottom headline `Developer Options` to see your 40 character API-Key.


Setting up a connection with your SyncSketch projects is as easy as following. 

    from syncsketch import SyncSketchAPI
    
    username = "username"
    api_key = "api-key-123"
    
    s = SyncSketchAPI(username, api_key)
    
    s.isConnected()

If you got a `200` response, you successfully connected to the syncsketch server! You can proceed with the next examples. We will skip the setup code for the next examples and the snippets will rely on each other, so make sure you run them one by one.


##### 1) Choose your account

Before we can create/read/update/delete reviews, we need to select an account

    accounts = s.getAccounts()
    firstAccount = accounts['objects'][0]

##### 2) Create a project

Let's create a project with the selected account

    project = s.addProject(firstAccount.id, 'DEV Example Project', 'DEV API Testing')

This creates a new Project called `Dev Example Project` with the description `DEV API Testing`


##### 3) Create a review

We can now add a Review to our newly created Project using it's `id`

    review = s.addReview(project['id'],'DEV Review (api)','DEV Syncsketch API Testing')


##### 4) Get list of reviews


    print(s.getReviewsByProjectId(project['id'])


##### 5) Upload a review item

You can upload a file to the created review with the review id, we provided one example file in this repo under `examples/test.webm` for testing.

    itemData = s.addMedia(review['id'],'examples/test.webm')


If all steps were successfull, you should see the following in the web-app. 

![alt text](https://github.com/syncsketch/python-api/blob/documentation/examples/ressources/exampleResult.jpg?raw=true)

### Additional Examples

##### Adding a user to the project
    addedUsers = s.addUsers(firstProjectId,[{'email':'test@syncsketch.com','permission':'viewer'}])
    print(addedUser)


##### Traverse all Reviews
    projects = s.getProjects()
    for project in projects['objects']:
        print(project)


##### Traverse all Reviews
    tree_data = s.getTree(withItems = True)
    for account in tree_data:
        for project in account['projects']:
            if project['active'] == 1:
                print project['name']
                for review in project['reviews']:
                    for item in review['items']:
                        mediaid = item['id']
                        medianame = item['name']
                        print '\t %s:\t%s'%(mediaid, medianame)

You might wan't to see some minimal examples on how an integration would work with a 3rd party API. 

 [1) Integrate Syncsketch into Shotgun with a column](https://github.com/syncsketch/python-api/tree/documentation/examples/SyncsketchColumnInShotgun)
