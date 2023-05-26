pip install spotipy

# getpass is a secure way to prompt the user for their information without displaying it
import getpass
# spotipy provides access to Spotify platform features
import spotipy
# authenticates and authorizes access to Spotify API using client credentials
from spotipy.oauth2 import SpotifyClientCredentials
# pprint formats and displays information in a more organized manner
import pprint
# numpy provides functions
import numpy as np
# plotting library that creates different charts and graphs
import matplotlib.pyplot as plt
# creates simplified interface for better visuals
import seaborn as sns

# get_spotify is used to get access to the Spotify API
# it asks the user for their client ID and client secret and attempts to connect to Spotify using those credentials
def get_spotify():

    # loop will give the user three attempts to enter the correct credentials
    for tries in range(1,4):
        # prompts the user to enter their client ID and then their client secret securly with getpass     
        spotify_client_id = input("Please enter your client ID: ")
        spotify_client_secret = getpass.getpass("Please enter your client secret: ")

        # try to connect to Spotify using the provided credentials
        try:
            sp = spotipy.Spotify(auth_manager=SpotifyClientCredentials(client_id=spotify_client_id,
                                                            client_secret=spotify_client_secret))
            
            # check if the connection is successful by accessing available markets
            # and returns Spotify object if successful
            sp.available_markets()
            return sp

        except spotipy.oauth2.SpotifyOauthError as ex:
                # if there's an error connecting to Spotify, display an error message
                print(f"Attempt {tries} of 3: Could not connect to Spotify."
                    "Please check your client ID and client secret")       

    # return "none" if all 3 attempts fail                
    return None

sp = get_spotify()
# specifically accesses the 'categories' from Spotify, which provides a list of available categories
sp.categories

# get_categories retrieves a list of categories available on Spotify
# takes a 'sp' as a parameter
def get_categories(sp):
    
    # retrieves category data from Spotify API, specifying parameters like country, limit, etc.
    categories_data = sp.categories(country='US', locale=None, limit=20, offset=0)

    # extracts the list of categories from the retrieved data
    categories = categories_data.get('categories').get('items')

    # creates and returns a dictionary to match category IDs to category
    return {category['id']: category['name'] for category in categories}

# calls the get_categories function using 'sp' to retrieve the available categories
categories = get_categories(sp)

# prints the available categories
print('Available categories:')
print("-" * 45)

for category_id, category_name in categories.items():
    print(f"{category_id}: {category_name}")

# prompts the user to input two categories of their choice
print('\nPick 2 categories!')
genre1 = input("Category 1: ")
genre2 = input("Category 2: ")

# creates an empty dictionary to store the selected categories
selected_categories = {}

# iterates through the available categories and checks if the user's inputs match any category names
# if a match is found, adds the category to the selected_categories dictionary
for category_id, category_name in categories.items():
    if category_name.lower() == genre1.lower():
        selected_categories[category_id] = category_name
    if category_name.lower() == genre2.lower():
        selected_categories[category_id] = category_name

# checks if exactly two categories were selected
if len(selected_categories) == 2:
    # if two categories were selected, prints the selected categories
    print('\nSelected categories:')
    for category_id, category_name in selected_categories.items():
        print(f"Category Name: {category_name}, Category ID: {category_id}")

else:
    # if one or both categories were not found, displays an error message
    print("One or both of the input categories were not found.")
    
# process_category processes a specific category by retrieving information about its playlists
# takes 'sp' and category ID as parameters
def process_category(sp, category_id):

    # retrieves a list of playlist IDs for the given category
    playlist_ids = get_playlists(sp, category_id)

    # creates an empty list to store information about playlists
    playlists = []

    # iterates through each playlist ID in the playlist_ids list
    for playlist_id in playlist_ids:
        # retrieves detailed information about the playlist using its ID
        playlist_data = sp.playlist(playlist_id)

        # processes the tracks within the playlist to calculate average duration, explicitness, and popularity
        avg_duration, avg_explicit, avg_popularity = process_tracks(sp, playlist_id)

        # creates a dictionary with information about the playlist and appends it to the playlists list
        playlists.append ({
            'id': playlist_data['id'],
            'name': playlist_data['name'],
            'followers': playlist_data['followers']['total'],
            'track_count': playlist_data['tracks']['total'],
            'avg_duration': avg_duration,
            'avg_explicit': avg_explicit,
            'avg_popularity': avg_popularity})

    # returns the list of playlists
    return playlists
    
# get_playlists retrieves the playlist IDs for all the playlists within a specific category
# takes 'sp' and category ID as parameters
def get_playlists(sp, category_id):

    # retrieves playlist data for the given category, specifying parameters like country, limit, etc.
    playlists_data = sp.category_playlists(category_id=category_id, country='US', limit=50, offset=0)

    # extracts the list of playlists from the retrieved data
    playlists = playlists_data['playlists']['items']

    # creates a list to extract the playlist IDs from the list of playlists
    # and returns list of playlist IDs
    return [playlist['id'] for playlist in playlists]
    
# process_tracks processes the tracks within a playlist and calculates the average duration, explicitness, and popularity
# takes 'sp' and playlist ID as parameters
def process_tracks(sp, playlist_id):

    # retrieves track data for the given playlist
    tracks_data = sp.playlist_tracks(playlist_id)
    tracks = tracks_data['items']

    # initializes variables to store cumulative values for duration, explicitness, popularity, and track count
    total_duration = 0
    total_explicit = 0
    total_popularity = 0
    num_tracks = len(tracks)

    # iterates through each track in the playlist and accumulates the values for duration, explicitness, and popularity
    for track in tracks:
        total_duration += track['track']['duration_ms']
        total_explicit += int(track['track']['explicit'])
        total_popularity += track['track']['popularity']

    # calculates the average duration, explicitness, and popularity by dividing the cumulative values by the number of tracks
    avg_duration = total_duration / num_tracks
    avg_explicit = total_explicit / num_tracks
    avg_popularity = total_popularity / num_tracks

    # returns the calculated average duration, explicitness, and popularity
    return avg_duration, avg_explicit, avg_popularity
    
# compare_categories creates a visual analysis of the differences between the categories
def compare_categories(categories):

    category = [] # category names
    explicit = [] # average explicitness of playlists
    followers = [] # average number of followers (thousands)
    duration = [] # average duration of playlists (seconds)
    popularity = [] # average popularity of playlists
    tracks = [] # number of tracks in each playlist

    # iterates over each category and its playlists
    for category_tuple in categories:
        category_name = category_tuple[0] # get category name
        playlist_list = category_tuple[1] # get list of playlists

        # iterate over each playlist
        for playlist in playlist_list:
            # append playlist attributes to given lists
            category.append(category_name)
            tracks.append(playlist.get('track_count',0))
            followers.append(playlist.get('followers',0) / 1000)
            duration.append(playlist.get('avg_duration',0) / 1000)  
            popularity.append(playlist.get('avg_popularity',0))
            explicit.append(playlist.get('avg_explicit',0))

    # compares tracks
    plt.figure(figsize=(10, 6))
    sns.boxplot(x=category, y=tracks)
    plt.title('Track Count Comparison')
    plt.show()

    # compares followers
    plt.figure(figsize=(10, 6))
    sns.boxplot(x=category, y=followers)
    plt.title('Followers Comparison')
    plt.show()

    # compares average duration
    plt.figure(figsize=(10, 6))
    sns.boxplot(x=category, y=duration)
    plt.title('Average Duration Comparison')
    plt.show()

    # compares popularity
    plt.figure(figsize=(10, 6))
    sns.boxplot(x=category, y=popularity)
    plt.title('Average Popularity Comparison')
    plt.show()

    # compares explicitness
    plt.figure(figsize=(10, 6))
    sns.boxplot(x=category, y=explicit)
    plt.title('Average Explicitness Comparison')
    plt.show()

category_playlists = []

for category_id, category_name in selected_categories.items():
    category_playlists.append((category_name, process_category(sp, category_id)))

compare_categories(category_playlists)
