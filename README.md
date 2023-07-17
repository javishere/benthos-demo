### Benthos

## Initialization

1. Init environment:  
   `docker-compose -f docker/docker-compose.yaml`
2. Create generator:  
   `curl http://localhost:4195/streams/match-simulator -X POST --data-binary @configs/tennis-match/1-config_generate_points_results.yaml`
3. See kafka-topic:
   `kafka-console-consumer --bootstrap-server localhost:9092 --topic benthos.stats`
4. See stats:  
   `curl -X GET http://localhost:4195/streams/match-simulator/stats | jq`
5. Split in statType (why? maybe you want to verify the structure of the of the messages without losing the incorrect ones):  
   `curl http://localhost:4195/streams/splitter -X POST --data-binary @configs/tennis-match/2-config_filter_stats.yaml`
6. See new topics:  
   `kafka-topics --bootstrap-server localhost:9092   --list`
7. Consumer one topic:  
   `kafka-console-consumer --bootstrap-server localhost:9092 --topic benthos.scorer`
8. Connect to redis:  
   `docker run -it --rm --network docker_default redis redis-cli -h redis`
9. Check there is no keys:  
    `keys *`
10. Create Points scorer stream:  
    `curl http://localhost:4195/streams/scorer -X POST --data-binary @configs/tennis-match/3-config_scorer_points_add.yaml`
11. 