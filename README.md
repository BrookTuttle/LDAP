from ldap3 import Server, Connection, ALL, MODIFY_REPLACE

ldap_server = Server('ldap://ldap.example.com', get_info=ALL)
ldap_user = 'cn=admin,dc=example,dc=com'
ldap_password = 'your_password'

conn = Connection(ldap_server, ldap_user, ldap_password, auto_bind=True)

search_base = 'ou=users,dc=example,dc=com'
search_filter = '(cn=John Doe)'
conn.search(search_base, search_filter, attributes=['cn', 'mail'])

if conn.entries:
    conn.modify(conn.entries[0].entry_dn, {'mail': [(MODIFY_REPLACE, ['new_email@example.com'])]})
    print('Email updated successfully.')
else:
    print('User not found.')

conn.unbind()
