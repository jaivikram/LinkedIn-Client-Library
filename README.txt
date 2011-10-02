For specific documentation of this Fork, scroll down below to the section - "Search Businesses"

===========================

This branch now includes a fair number of pull requests from PhilGo20 to help with the updates LinkedIn made to their search API, as well as some minor bug fixes.

Big thanks go out to Phil for helping out!

See the docs here:
    http://developer.linkedin.com/docs/DOC-1191#
    
Here's an example of how to use the search:

from liclient import LinkedInAPI
api = LinkedInAPI(API_KEY, SECRET_KEY)
params = {'first-name': 'John', 'last-name': 'Smith'}
field_selector_string = '(people:(id,first-name,last-name,headline,public-profile-url),num-results)'
results = api.search(ACCESS_TOKEN, params, field_selector_string)
# do stuff with results

Dependencies:
    lxml
    httplib2 (for OAuth)

Installable via:

        python setup.py install

To get started:

1.  First, of course, we need to instantiate the API object with our consumer key and secret:

     consumer_key = 'mykey'
     consumer_secret = 'mysecret'
     APIClient = LinkedInAPI(consumer_key, consumer_secret)

2.  The first step for a new user is to retrieve a request token.  This is done like so:

     request_token = APIClient.get_request_token()

3.  Then, we generate the URL to send the user to for authentication (I know the first line is a little ugly, I will probably simplify this soon):

     authorization_url = APIClient.base_url + APIClient.authorize_path
     url = "%s?oauth_token=%s" % (authorization_url, request_token['oauth_token'])

4.  Once the user has authenticated, you will need to collect the oauth_verifier returned by the LinkedIn server in the URL.  This can be done
     either by having the user type it in themselves or collecting the URL argument on the redirect from LinkedIn.  However you decided to get it,
     this is how you use it:

     access_token = APIClient.get_access_token(request_token, oauth_verifier)

That's it!  You can use this access token for every request this particular user makes, for as long as they have authorized you to use it.

=========================================================================================

Search Businesses
-----------------

def search_company(self, access_token, universal_name=None, email_domain=None)
- where atleast one of email_domain or universal_name is required.

def get_company_profile(self, access_token, rid, selectors=[], **kwargs)
- where rid (linkedin resource id) of the company is required.

e.g.:

>>> api.search_company(access_token, email_domain='apple.com')

[{'rid': '162479', 'name': 'Apple Inc.'}, {'rid': '1276', 'name': 'Apple Retail'}]

>>> o = api.get_company_profile(access_token, 1337)

>>> print(o.name, o.ticker, o.website_url, o.twitter_id, o.logo_url, o.industry)
('LinkedIn', 'LNKD', 'http://www.linkedin.com', 'linkedin', 'http://media.linkedin.com/mpr/mpr/p/3/000/0c2/1d7/1894403.png', 'Internet')

>>> from pprint import pprint

>>> pprint(o.locations)
[{'city': 'Mountain View',
  'country_code': 'us',
  'is_headquarters': 'true',
  'phone': None,
  'state': 'CA',
  'street1': '2029 Stierlin Court',
  'street2': None,
  'zipcode': '94043'},
 {'city': 'Omaha',
  'country_code': 'us',
  'is_headquarters': 'false',
  'phone': '(402) 452-2320',
  'state': 'NE',
  'street1': '2126 N 117th Ave',
  'street2': None,
  'zipcode': '68164'},
 {'city': 'Belmont',
  'country_code': 'us',
  'is_headquarters': 'false',
  'phone': None,
  'state': 'MA',
  'street1': '15 Shady Brook Lane',
  'street2': None,
  'zipcode': '02478'},
 {'city': 'San Francisco',
  'country_code': 'us',
  'is_headquarters': 'false',
  'phone': None,
  'state': 'CA',
  'street1': '450 Sansome Street',
  'street2': 'Suite 650',
  'zipcode': '94111'},
 {'city': 'Belmont',
  'country_code': 'us',
  'is_headquarters': 'false',
  'phone': None,
  'state': 'MA',
  'street1': '15 Shady Brook Lane',
  'street2': None,
  'zipcode': '94301'}]
