in `doc/tutorials/configuration-file/onos.json` is avalable the configuration file for SDN-IP in ONOS

this command must be issued from repository root directory in order to setup the configuration at start-up of ONOS controller (after you change configuration you must restart controller in order to see it works)
```
curl --user onos:rocks -X POST -H "Content-Type: application/json" http://localhost:8181/onos/v1/network/configuration/ -d @doc/tutorials/configuration-file/onos.json
```
be sure that config module is activate in order to post the new configuration
```
app activate org.onosproject.config
```
S1 -> of:00007a037f62e943
S2 -> of:000036ce1ed41c4a
S3 -> of:000076ded54afc45