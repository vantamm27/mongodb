# mongodb
# create user root
use admin
db.createUser( { user: "root", pwd: "pwd", roles: [ "root"] } )

# create key
openssl rand -base64 756 > mongodb1/mongo-keyfile


# add create replica

rs.initiate(
  {
    _id : 'vngcsRs',
    members: [
      { _id : 0, host : "172.24.2.1:27017","priority": 100 },
      { _id : 1, host : "172.24.2.2:27017","priority": 99 },
      { _id : 2, host : "172.24.2.3:27017","priority":  98 }
    ]
  },{ force: true }
)

rs.reconfig(
  {
    _id : 'vngcsRs',
    protocolVersion: 1,
    members: [
      { _id : 0, host : "172.24.2.1:27017","priority": 1 },
      { _id : 1, host : "172.24.2.2:27017","priority": 0.5 },
      { _id : 2, host : "172.24.2.3:27017","priority": 0.5 }
    ]
  },{ force: true }
)

# add member
rs.add('172.24.2.2:27017')


