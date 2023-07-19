## Setup
`make build` 


## Configuration
`spade.toml` contains the configuration for the webapi.
By default it will look in `~/spade.toml`


`touch ~/spade.toml`

```toml
//TBD
```

## Dependencies

### Database
You need to have a postgres server available for the spade webapi db. 

Here's how to run one in docker, good for dev purposes:
```bash
docker run --name spade-postgres -p 5432:5432 -e POSTGRES_PASSWORD=password -d postgres:14.7
```

Tip: To connect to it for troubleshooting:
```bash
psql postgres://postgres:password@localhost:5432/
```

Set up the DB Schema with the following command, from the root of this repo:
```bash
psql postgres://postgres:password@localhost:5432/ < misc/pg_schema.sql 
```

## Running
Run the webapi

```bash
./bin/spade-webapi --pg-connstring postgres://postgres:password@localhost:5432/
```

Flags:

```bash
   --webapi-listen-address value  (default: "localhost:8080")
   --lotus-api-lite value         (default: "https://api.chain.love")
   --lotus-api-heavy value        (default: "http://localhost:1234")
   --lotus-api-heavy-token value  (default:   {{ private, read from config file }}  )
   --lotus-lookback-epochs value  (default: 10 epochs / 300s)
   --pg-connstring value          (default: "postgres:///dbname?user=username&password=&host=/var/run/postgresql")
   --pg-metrics-connstring value  (default: defaults to pg-connstring)
   --help, -h                     show help (default: false)
```


Verify that it's working 

```bash
curl http://localhost:8080/sp/
```




## rough notes

```bash
curl --request GET \
  --url http://localhost:8080/sp/pending_proposals \
  --header 'Authorization: FIL-SPID-V0 3050126;f01886797;tv++L96Tquuqo3n+DjBpBs/s+1lrljVKefHDQADOtq+SOR5rqpby5HBcSFb2mHKeC2+Jlxa0lma1Hvl4DQ78BMjYH3TaE7CuB8amhwIpb1yNFHDQLS2CEvuyPZgWiT0y'
```

is failing with 

```bash
error":"RPC client error: sendRequest failed: Post \"http://localhost:1234/rpc/v0\": dial tcp [::1]:1234: connect: connection refused","status":500,"took":"22.666542ms","sp":"","bytes_in":0,"bytes_out":36,"op":"GET localhost:8080/sp/pending_proposals","remote_ip":"127.0.0.1","user_agent":"insomnia/2023.4.0"}
```

Looks like we need to get the fullnode connecting.