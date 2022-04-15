# Parking App

## Table of Contents
1. [Overview](#Overview)
1. [Product Spec](#Product-Spec)
1. [Wireframes](#Wireframes)
2. [Schema](#Schema)

## Overview
### Description
This app will be able to used by commuters or travelers who are looking for places to park where it is hard to find parking. This includes urban areas such as cities where the population is only increasing. By pulling in data based on the user's current location, or one they that input, the app will be able to show the user a list of parking facilities that are most convenient to park at.  

### App Evaluation
[Evaluation of your app across the following attributes]
- **Category:** Travel/Productivity
- **Mobile:** Maps, Location, Real-Time
- **Story:** This app will be able to be used by commuters and travelers who are looking for parking spots around their areas and it will be highly useful. This product would be received well.
- **Market:** Most everyone will or has needed to find a parking spot in their lifetime especially in the US, so I think that this app will be pretty useful especially in places with high population density.
- **Habit:** Whenever people are in need of parking or need to find a place to park, which may be often, this app will help guide them. If we make the UI appealing enough and the functionality fast, we will be able to create a good user experience.
- **Scope:** This app will be pretty technically challenging, however we do have different aspects and features of the app that we do have in mind to complete that are feasible, and then others that will make the app better albeit are not as feasible. The stripped down version will still be interesting to build. 

## Product Spec

### 1. User Stories (Required and Optional)

**Required Must-have Stories**

* [ ] - Create login/register/continue as guest screen
* [ ] - Input or grab the location for parking
* [ ] - Spit out a list of parking spots, their addresses, and the distance from your current location
* [ ] - User can update their password
* [ ] - User can logout from their account
* [ ] - User stays logged in when exiting app
* [x] - Setup backend

**Optional Stories**

* [ ] - Display parking prices and if it is meter/less
* [ ] - Automatically find location using user location
* [ ] - Display locations on MapKit (or similar)
* [ ] - Login to find user preferred parking

### 2. Screen Archetypes
* Login
   * Login to account or continue as guest
* Register - User signs up or logs in
   * When opening the app, the user is prompted to login if they have an account, register if they do not have an account, or to continue as a guest.
* Creation
   * Input the location(zip code or address)for parking
* Stream
   * Spit out a list of parking spots and their addresses in a table
* Map View
    * View locations in a map
* Settings
    * Change your password

### 3. Navigation

**Tab Navigation** (Tab to Screen)

* [fill out your first tab]
* [fill out your second tab]
* [fill out your third tab]

**Flow Navigation** (Screen to Screen)

* Login/Register screen
   * Create login/register/continue as guest screen
   * ...
* Search Parking Screen
   * Input or grab the location for parking
* List View
    * View parking locations as a list
* Map View
    * View parking locations on a map
* Settings View
    * Users can change password
* Logout
    * Brings user back to login screen

## Wireframes
[Add picture of your wireframes in this section]
<iframe style="border: 1px solid rgba(0, 0, 0, 0.1);" width="800" height="450" src="https://www.figma.com/embed?embed_host=share&url=https%3A%2F%2Fwww.figma.com%2Ffile%2FwOTzwrtGpIHSoKkD0b0WQM%2FParkMe%3Fnode-id%3D0%253A1" allowfullscreen></iframe>

### [BONUS] Digital Wireframes & Mockups

### [BONUS] Interactive Prototype
<img src='https://github.com/Alankvuong/ParkMe/blob/main/ParkMe-App-Prototype.gif?raw=true' />
https://www.figma.com/proto/wOTzwrtGpIHSoKkD0b0WQM/ParkMe?node-id=32%3A203&scaling=scale-down&page-id=0%3A1&starting-point-node-id=1%3A6

## Schema 
[This section will be completed in Unit 9]
### Models

| Model: User       |                 |                                          |
|-------------------|-----------------|------------------------------------------|
| Property          | Type            | Description                              |
| favoriteLocations | Array[Location] | A list of favorite locations for parking |
| history           | Array[Location] | The history of locations they have used  |
|                   |                 |                                          |
| Model: Location   |                 |                                          |
| Property          | Type            | Description                              |
| name              | String          | The name of the parking spot             |
| price             | Double          | Price of a parking spot at the location  |
| address           | String          | Address of Parking lot                   |
| phoneNo           | String          | Phone number of lot                      |
| description       | String          | Description of Parking Lot               |
| imageUrl          | String          | URL of parking lot image                 |
| hours             | bool            | Is the parking lot open?                 |
| latitude          | Double          | Latitudinal position of location         |
| longitude         | Double          | Longitudinal position of location        |
| starRating        | Double          | Rating of location                       |

### Networking

- Login Screen
    - (Create/POST) a User
    ```swift
    PFObject *pfUser = [PFObject objectWithClassName:"User"]
    pfUser["favoriteLocations"] = []
    pfUser["history"] = []

    pfUser.saveInBackground { (succeeded, error)  in
        if (succeeded) {
            // The object has been saved.
        } else {
            // There was a problem, check error.description
        }
    }
    ```
- Home Feed
    - (Read/GET) List of parking locations nearby

- Location Page
    - (Read/GET) Location and its data values
    - (Update/PUT) Locations into History
    ```swift
    let location = {SELECTEDLOCATIONHERE}
    let favorite = true;
    let query = PFQuery(className:"User")
    query.getObjectInBackground(withId: {USERIDHERE}) { (pfUser: PFObject?, error: Error?) in
        if let error = error {
            print(error.localizedDescription)
        } else if let pfUser = pfUser {
            pfUser["history"] += {LOCATIONHERE}
            pfUser.saveInBackground()
        }
    }
    ```

    - (Update/PUT) Locations into Favorites feed
    ```swift
    let location = {SELECTEDLOCATIONHERE}
    let favorite = true;
    let query = PFQuery(className:"User")
    query.getObjectInBackground(withId: {USERIDHERE}) { (pfUser: PFObject?, error: Error?) in
        if let error = error {
            print(error.localizedDescription)
        } else if let pfUser = pfUser {
            pfUser["favorites"] += {LOCATIONHERE}
            pfUser.saveInBackground()
        }
    }
    ```


- History Feed
    - (Read/GET) List of location histories

    ```swift
    // iOS
    // (Read/GET) Query all posts where user is author
    let query = PFQuery(className:"History")
    query.findObjectsInBackground { (posts: [PFObject]?, error: Error?) in
       if let error = error {
          print(error.localizedDescription)
       } else if let locations = locations {
          print("Successfully retrieved \(posts.count) posts.")
          // TODO: Do something with posts...
       }
    }
    ```


    - (Delete) certain locations off the feed

    ```swift
    let query = PFQuery(className:"History")
    query.deleteAll(inBackground: [{LOCATIONTODELETE}]) { (succeeded, error) in
        if (succeeded) {
            // The array of objects was successfully deleted.
        } else {
            // There was an error. Check the errors localizedDescription.
        }
    }
    ```

   
   
- Favorites Feed
    - (Read/GET) List of location favorites
    ```swift
    // iOS
    // (Read/GET) Query all posts where user is author
    let query = PFQuery(className:"Favorites")
    query.findObjectsInBackground { (posts: [PFObject]?, error: Error?) in
       if let error = error {
          print(error.localizedDescription)
       } else if let locations = locations {
          print("Successfully retrieved \(posts.count) posts.")
          // TODO: Do something with posts...
       }
    }
    ```

    - (Delete) certain locations off the feed
    
    ```swift
    let query = PFQuery(className:"Favorites")
    query.deleteAll(inBackground: [{LOCATIONTODELETE}]) { (succeeded, error) in
        if (succeeded) {
            // The array of objects was successfully deleted.
        } else {
            // There was an error. Check the errors localizedDescription.
        }
    }
    ```


