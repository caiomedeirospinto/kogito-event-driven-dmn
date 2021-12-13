# kogito-event-driven-dmn

# Local Setup

## Use docker-compose

Use the `docker-compose.yml` file to start Zookeeper, Kafka, and Kafdrop.

```
docker-compose up -d
```

## Start the Decision Service

### Development

Run the application using Spring Boot

```
mvn clean spring-boot:run
```

### JVM

Compile and run executable JAR

```
mvn clean package

java -jar target/kogito-event-driven-dmn.jar
```

# Testing

## Produce Request Message to Kafka Request Topic

Send a request to the topic `truck-to-product-line-requests`

### Sample Request

```
{
  "specversion": "1.0",
  "id": "a89b61a2-5644-487a-8a86-144855c5dce8",
  "source": "SomeEventSource",
  "type": "DecisionRequest",
  "subject": "TheSubject",
  "kogitodmnmodelname": "Truck To Product Line",
  "kogitodmnmodelnamespace": "https://kiegroup.org/dmn/_94DEED63-0DB4-41CB-BBDE-F1E49F3FFBF6",
  "data": {
    "Truck": {
      "ID": 0,
      "Material": {
        "Density": 3,
        "Type": "Pine"
      }
    },
    "Product Lines": [
      {
        "Target Density": 5,
        "Minimum Density": 3,
        "Maximum Density": 9,
        "Current Density": 4
      },
      {
        "Target Density": 5,
        "Minimum Density": 3,
        "Maximum Density": 9,
        "Current Density": 3
      },
      {
        "Target Density": 5,
        "Minimum Density": 3,
        "Maximum Density": 9,
        "Current Density": 7
      }
    ]
  }

}
```

## Consume Message from Response Topic

The response will be sent to the Kafka Topic `truck-to-product-line-responses`.

### Sample Response

```
{
   "specversion": "1.0",
   "id": "4c886fdd-c40c-4023-bf4b-619522b1bcc9",
   "source": "Truck+To+Product+Line",
   "type": "DecisionResponse",
   "subject": "TheSubject",
   "kogitodmnmodelnamespace": "https://kiegroup.org/dmn/_94DEED63-0DB4-41CB-BBDE-F1E49F3FFBF6",
   "kogitodmnmodelname": "Truck To Product Line",
   "data": {
      "Matched Product Lines": [
         {
            "Is Truck Matched": true,
            "Score": 2,
            "Proposed Density": 7,
            "Current Density": 4,
            "Target Density": 5,
            "Minimum Density": 3,
            "Maxium Density": 9
         },
         {
            "Is Truck Matched": true,
            "Score": 1,
            "Proposed Density": 6,
            "Current Density": 3,
            "Target Density": 5,
            "Minimum Density": 3,
            "Maxium Density": 9
         }
      ],
      "Evaluate Match": "function Evaluate Match( Truck, Product Line )",
      "Best Match": {
         "Is Truck Matched": true,
         "Score": 1,
         "Proposed Density": 6,
         "Current Density": 3,
         "Target Density": 5,
         "Minimum Density": 3,
         "Maxium Density": 9
      },
      "Truck": {
         "Material": {
            "Type": "Pine",
            "Density": 3
         },
         "ID": 0
      },
      "Product Lines": [
         {
            "Score": null,
            "Is Truck Matched": null,
            "Proposed Density": null,
            "Current Density": 4,
            "Maximum Density": 9,
            "Target Density": 5,
            "Minimum Density": 3
         },
         {
            "Score": null,
            "Is Truck Matched": null,
            "Proposed Density": null,
            "Current Density": 3,
            "Maximum Density": 9,
            "Target Density": 5,
            "Minimum Density": 3
         },
         {
            "Score": null,
            "Is Truck Matched": null,
            "Proposed Density": null,
            "Current Density": 7,
            "Maximum Density": 9,
            "Target Density": 5,
            "Minimum Density": 3
         }
      ]
   }
}
```