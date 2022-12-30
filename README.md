esp32-srp
============
Derived from https://github.com/dwimberger/mbedtls-csrp

esp32-srp is a minimal C implementation of the [Secure Remote Password
protocol](http://srp.stanford.edu/). The project consists of a single
C file and is intended for direct inclusion into utilizing programs. 
It's only dependency is mbedtls (https://github.com/ARMmbed/mbedtls).
It can be dropped as is as a component in an ESP-IDF project.

SRP Overview
------------

SRP is a cryptographically strong authentication
protocol for password-based, mutual authentication over an insecure
network connection.

Unlike other common challenge-response autentication protocols, such
as Kereros and SSL, SRP does not rely on an external infrastructure
of trusted key servers or certificate management. Instead, SRP server
applications use verification keys derived from each user's password
to determine the authenticity of a network connection.

SRP provides mutual-authentication in that successful authentication
requires both sides of the connection to have knowledge of the
user's password. If the client side lacks the user's password or the
server side lacks the proper verification key, the authentication will
fail.

Unlike SSL, SRP does not directly encrypt all data flowing through
the authenticated connection. However, successful authentication does
result in a cryptographically strong shared key that can be used
for symmetric-key encryption.

Entropy
-------

You need to take care of entropy to achieve cryptograhically sound random values.
For real world use, you should change the implementation in srp_crypto_random_init() to supply your own seed
values. Also make sure to add entropy sources to your mbedtls port.

Installation
------------

Extract or clone the module directly into the components directory of your project

Test
----

The test can be ran from the command line as a normal ESP-IDF project.
first,install esp32 build tools
https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/index.html#installation

second, Simply run ```idf.py -p /dev/tty.usbserial-0001(your esp32 device) flash  monitor``` to run on a connected esp32 board.

Usage
-----

Please look at the test source file for an example of interaction between SRP client and server

Test result
-----

Iteration: 1000 -------------------------------------------------------------------------------------------

I (579323) SRP_TEST: srp_new_server
I (579333) SRP_TEST: Usec srp_new_server: 678
I (579333) SRP: srp_new_server
I (579343) SRP_TEST: ====================== M1: Client -> Server -- 'SRP Start Request'
I (579343) SRP_TEST: srp_new_client
I (579353) SRP_TEST: Usec srp_new_client: 458
I (579353) SRP: srp_new_client
I (579363) SRP_TEST: ====================== M2: Server -> Client -- 'SRP Start Response'
I (579373) SRP_TEST: Server srp_set_username
I (579373) SRP_TEST: Usec server srp_set_username: 46
I (579383) SRP: srp_set_username
I (579383) SRP_TEST: Server srp_set_auth_password
I (579433) SRP_TEST: Usec server srp_set_auth_password: 40327
I (579433) SRP: srp_set_auth_password
I (579433) SRP_TEST: Server srp_gen_pub
I (579473) SRP_TEST: Usec server srp_gen_pub: 41949
I (579473) SRP: srp_set_auth_password
I (579473) SRP_TEST: ====================== M3: Client -> Server -- 'SRP Verify Request'
I (579483) SRP_TEST: Server srp_get_salt
I (579493) SRP_TEST: Usec client srp_set_params: 72
I (579493) SRP: srp_set_params
I (579493) SRP_TEST: Client srp_gen_pub
I (579543) SRP_TEST: Usec client srp_gen_pub: 39163
I (579543) SRP: srp_gen_pub
I (579543) SRP_TEST: Client srp_set_username
I (579543) SRP_TEST: Usec client srp_set_username: 48
I (579553) SRP: srp_set_username
I (579553) SRP_TEST: Client srp_set_auth_password
I (579563) SRP_TEST: Usec client srp_set_auth_password: 380
I (579563) SRP: srp_set_auth_password
I (579573) SRP_TEST: Client srp_compute_key
I (579693) SRP_TEST: Usec client srp_compute_key: 121805
I (579693) SRP: srp_compute_key
I (579693) SRP_TEST: ====================== M4: Server -> Client -- 'SRP Verify Response'
I (579703) SRP_TEST: Server srp_compute_key
I (579833) SRP_TEST: Usec server srp_compute_key: 126556
I (579833) SRP: srp_compute_key
I (579833) SRP_TEST: Server srp_verify_key
I (579843) SRP_TEST: Usec server srp_verify_key: 42
I (579843) SRP: srp_compute_key
I (579853) SRP_TEST: ====================== M5: Client -> Server -- 'Exchange Request'
I (579853) SRP_TEST: Client srp_verify_key
I (579863) SRP_TEST: Usec client srp_verify_key: 26
I (579863) SRP: srp_compute_key
I (579873) SRP_TEST: Authentication successful
I (579873) SRP_TEST: uSec server CPU: 209614661 (avg: 209614)
I (579883) SRP_TEST: uSec client CPU: 159781861 (avg: 159781)
I (579893) SRP_TEST: Total tests: 1000, successes: 1000, failures: 0
I (579903) SRP_TEST: uSec total: 579558278
I (579903) SRP_TEST: uSec total CPU: 369396522 (avg: 369396)
I (579903) SRP_TEST: uSec server CPU: 209614661 (avg: 209614)
I (579913) SRP_TEST: uSec client CPU: 159781861 (avg: 159781)
I (579923) SRP_TEST: Total tests: 1000, successes: 1000, failures: 0
