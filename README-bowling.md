
## Initialization
### Benthos

1. Init environment:  
   `docker-compose -f docker/docker-compose.yaml up -d`
2. Create points generator:  
   `curl http://localhost:4195/streams/turn -X POST --data-binary @configs/bowling-night/1-config_generate_dump_alert.yam`
3. See kafka-topic:  
   `kafka-console-consumer --bootstrap-server localhost:9092 --topic bowling_shots_results`
4. See stats:  
   `curl -X GET http://localhost:4195/streams/turn/stats | jq`
5. Split in statType (why? maybe you want to verify the structure of the of the messages without losing the incorrect ones):  
   ```
   curl http://localhost:4195/streams/player1 -X POST --data-binary @configs/bowling-night/1-config_player1.yaml
   curl http://localhost:4195/streams/player2 -X POST --data-binary @configs/bowling-night/2-config_player1.yaml
   curl http://localhost:4195/streams/player3 -X POST --data-binary @configs/bowling-night/3-config_player1.yaml
   ```
7.  Consumer scorer topic:  
   `kafka-console-consumer --bootstrap-server localhost:9092 --topic bowling.scorer`
6. See new topics:  
   `kafka-topics --bootstrap-server localhost:9092   --list`
7. See kafka-topic:  
   ` curl http://localhost:4195/streams/scorer -X PUT --data-binary @configs/bowling-night/4-confgi_scorer.yaml`
7.  Consumer scorer topic:  
   `kafka-console-consumer --bootstrap-server localhost:9092 --topic bowling_shots_results`



## Notes

* Inside Blobang: *this* is the input document and *root* is the output document.