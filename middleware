# -*- coding: utf-8 -*-

"""
This module is a Django middleware to provide oAuth2 authentication
Configuration steps:
    a) Add this file in your app directory
    b) Add the line bellow in setting.py
        MIDDLEWARE += ['<your_app_label>.middleware.oauth2']
    c) Add a link in the HTML login page
        <a href="{{ request.oauth2_authorize_url }}">Login with Oauth2</>
    d) Provide the values for the constants
    e) Write the body of the function "process_response()" accordingly to your needs
       and return a string URL to redirect the user
"""

import json
import requests
from django.http import HttpResponseRedirect

REDIRECT_URI = ''
CLIENTE_ID = ''
CLIENT_SECRET = ''
AUTHORIZE_URL = ''
ACCESS_TOKEN_URL = ''
USER_DATA_URL = ''

authorize_url = url = '{}?response_type=code&client_id={}&redirect_uri={}'.format(
    AUTHORIZE_URL, CLIENTE_ID, REDIRECT_URI
)
acess_token_url = '{}?grant_type=authorization_code&code={{}}&redirect_uri={}&client_id={}&client_secret={}'.format(
    ACCESS_TOKEN_URL, REDIRECT_URI, CLIENTE_ID, CLIENT_SECRET
)



def process_response(data):
    print(data)
    return '/admin/'


def oauth2(get_response):

    def middleware(request):
        request.oauth2_authorize_url = authorize_url
        if request.path == '/' and 'code' in request.GET:
            data = json.loads(
                requests.post(acess_token_url.format(request.GET.get('code')), verify=False).text
            )
            print(data)
            headers = {'Authorization': 'Bearer {}'.format(data.get('access_token')), 'x-api-key': CLIENT_SECRET}
            data = json.loads(
                requests.post(USER_DATA_URL, data={'scope': data.get('scope')}, headers=headers).text
            )
            return HttpResponseRedirect(process_response(data))
        response = get_response(request)
        return response

    return middleware
