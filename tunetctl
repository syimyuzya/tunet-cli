#! /usr/bin/env python
# coding=utf-8

# TUNet CLI
# Command line tool for Tsinghua University net login
# by SyiMyuZya <syimyuzya@gmail.com>

from __future__ import print_function, unicode_literals

import sys

from getopt import gnu_getopt, GetoptError
from getpass import getpass
from md5 import md5
from urllib import urlencode
from urllib2 import urlopen

URL_LOGIN = 'http://net.tsinghua.edu.cn/cgi-bin/do_login'
URL_LOGOUT = 'http://net.tsinghua.edu.cn/cgi-bin/do_logout'

# utilities
def check():
    '''
    Get online information of this connection.
    '''
    return urlopen(URL_LOGIN, b'action=check_online').read().split(',')

def login(username, password):
    '''
    Login with username and password(encrypted).
    
    This function does not need the raw password.
    '''
    return urlopen(URL_LOGIN, urlencode({
        'username':username,
        'password':password,
        'drop':'0',
        'type':'1',
        'n':'100'
    })).read()

def logout():
    '''
    Logout.
    '''
    return urlopen(URL_LOGOUT, b'').read()

# interface

def show_version():
    print('TUNet Command Line Tool',
          'version 0.0.1',
          '',
          'by SyiMyuZya <syimyuzya@gmail.com>',
          sep='\n',
          file=sys.stderr)

def show_usage():
    show_version()
    print('',
          'Usage:',
          ' no option:',
          '  Show the current connect status of this device.',
          ' -l --login:',
          '  Login.',
          ' -o --logout:',
          '  Logout.',
          ' -h --help',
          '  Print this message',
          sep='\n',
          file=sys.stderr)

def show_status():
    print('Current status:')
    status = check()
    if len(status)==1:
        print('  Not logged in.')
    else:
        print('  User: '+status[1],
              '  Usage: '+status[2]+'B',
              '  Time: '+status[4]+'s',
              '  (Unknown arg0): '+status[0],
              '  (Unknown arg3): '+status[3],
              sep='\n')

def do_login():
    print('User: ', end='')
    username = raw_input()
    password = md5(getpass()).hexdigest()
    res = login(username, password)
    print('Server response:', res)
    show_status()

def do_logout():
    res = logout()
    print('Server response:', res)

def main():
    try:
        opts, args = gnu_getopt(sys.argv[1:], 'hlo', ['help', 'login', 'logout'])
        if len(args):
            raise GetoptError('unexpected argument '+args[0])
        op = []
        for opt in opts:
            if opt[0]=='-h' or opt[0]=='--help':
                op.append('help')
            if opt[0]=='-l' or opt[0]=='--login':
                op.append('login')
            if opt[0]=='-o' or opt[0]=='--logout':
                op.append('logout')
        if len(op)==0:
            show_status()
        elif len(op)>1:
            raise GetoptError('can not perform multiple actions: '+repr(op))
        else:
            {'help':show_usage, 'login':do_login, 'logout':do_logout}[op[0]]()
    except GetoptError as e:
        print('Err: '+e.msg,
              'Use -h(--help) for help',
              sep='\n', file=sys.stderr)
    except KeyboardInterrupt:
        print('Canceled', file=sys.stderr)

if __name__=='__main__':
    main()

# vim: ts=4 sw=4 et
