
## Initialization
### Benthos

1. Init environment:  
   `docker-compose -f docker/docker-compose.yaml up -d`
2. Create points generator:  
   `curl http://localhost:4195/streams/points-gen -X POST --data-binary @configs/tennis-match/1-config_generate_points_results.yaml`
3. Create aces generator:  
   `curl http://localhost:4195/streams/aces-gen -X POST --data-binary @configs/tennis-match/2-config_generate_aces.yaml`
4. See kafka-topic:  
   `kafka-console-consumer --bootstrap-server localhost:9092 --topic benthos.stats`
5. See stats:  
   `curl -X GET http://localhost:4195/streams/match-simulator/stats | jq`
6. Split in statType (why? maybe you want to verify the structure of the of the messages without losing the incorrect ones):  
   `curl http://localhost:4195/streams/splitter -X POST --data-binary @configs/tennis-match/2-config_filter_stats.yaml`
7. See new topics:  
   `kafka-topics --bootstrap-server localhost:9092   --list`
8. Consumer one topic:  
   `kafka-console-consumer --bootstrap-server localhost:9092 --topic benthos.scorer`
9. Connect to redis:  
   `docker run -it --rm --network docker_default redis redis-cli -h redis`
10. Check there is no keys:  
    `keys *`
11. Create Points scorer stream:  
    `curl http://localhost:4195/streams/scorer -X POST --data-binary @configs/tennis-match/3-config_scorer_points_add.yaml`


## Notes

* Inside Blobang: *this* is the input document and *root* is the output document.