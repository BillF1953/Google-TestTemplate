#Google Test Template for CLion


Most (All) from https://github.com/snikulov/google-test-examples. 
Two Changes to compile on CLion using Cygwin:

Main Cmake: add_definitions(-std=gnu++0x) to set compiler to GNU.

Gtest Cmake: Install Download is enabled. CLion requires a physical copy of gtest in project directory. 
