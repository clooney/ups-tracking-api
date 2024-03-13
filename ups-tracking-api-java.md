UPS Tracking API - Java
================================
Use Java to track UPS shipments with UPS Tracking API.

Features
--------
- Real-time UPS tracking.
- Batch UPS tracking.
- Other features to manage your UPS tracking.

Installation
------------

**Maven**

    $ <dependency>
    $ <groupId>io.github.trackingmores</groupId>
    $ <artifactId>trackingmore-sdk-java</artifactId>
    $ <version>1.0.3</version>
    $ </dependency>

**Gradle**

    $ implementation "io.github.trackingmores:trackingmore-sdk-java:1.0.3"

Quick Start
----------
Get the API key:

To use this API, you need to generate your API key.

- <a href="https://admin.trackingmore.com/developer/apikey" target="_blank" rel="noreferrer">
  Click here</a> to access TrackingMore admin.

- Go to the "Developer" section.

- Click "Generate API Key".

- Give a name to your API key, and click "Save" .


Then, start to track your UPS shipments.

Usage
----------

Create a tracking (Real-time tracking):

    package com.trackingmore.example.ups;
    
    import com.trackingmore.TrackingMore;
    import com.trackingmore.exception.TrackingMoreException;
    import com.trackingmore.model.TrackingMoreResponse;
    
    import java.io.IOException;
    import java.util.List;
    
    public class CreateUPSTrackingExample {
    
        public static void main(String[] args) {
            try {
                String apiKey = "your api key";
                TrackingMore trackingMore = new TrackingMore(apiKey);
                
                CreateTrackingParams createTrackingParams = new CreateTrackingParams();
                createTrackingParams.setTrackingNumber("802608005721255486");
                createTrackingParams.setCourierCode("ups");
                TrackingMoreResponse<Tracking> result = trackingMore.trackings.CreateTracking(createTrackingParams);
                System.out.println(result.getMeta().getCode());
                if(result.getData() != null){
                   Tracking trackings = result.getData();
                   System.out.println(trackings);
                   System.out.println(trackings.getTrackingNumber());
                }
            } catch (TrackingMoreException e) {
                System.err.println("error：" + e.getMessage());
            } catch (IOException e) {
                System.err.println("error：" + e.getMessage());
            }
        }
    
    }


Create trackings (Max. 40 tracking numbers create in one call):

    package com.trackingmore.example.ups;
    
    import com.trackingmore.TrackingMore;
    import com.trackingmore.exception.TrackingMoreException;
    import com.trackingmore.model.TrackingMoreResponse;
    
    import java.io.IOException;
    import java.util.List;
    
    public class CreateUPSTrackingsExample {
    
        public static void main(String[] args) {
            try {
                String apiKey = "your api key";
                TrackingMore trackingMore = new TrackingMore(apiKey);
                
                List<CreateTrackingParams> paramsList = new ArrayList<>();
                
                CreateTrackingParams createTrackingParams1 = new CreateTrackingParams();
                createTrackingParams1.setTrackingNumber("1ZC6G8280303220451");
                createTrackingParams1.setCourierCode("ups");
                
                CreateTrackingParams createTrackingParams2 = new CreateTrackingParams();
                createTrackingParams2.setTrackingNumber("1ZR8E2200404839077");
                createTrackingParams2.setCourierCode("ups");
                
                paramsList.add(createTrackingParams1);
                paramsList.add(createTrackingParams2);
                
                TrackingMoreResponse<BatchResults> result = trackingMore.trackings.BatchCreateTrackings(paramsList);
                System.out.println(result.getMeta().getCode());
                BatchResults batchResults = result.getData();
                if(result.getData() != null){
                    System.out.println(batchResults);
                    for (BatchItem batchItem : batchResults.getSuccess()) {
                        String trackingNumber = batchItem.getTrackingNumber();
                        String courierCode = batchItem.getCourierCode();
                        System.out.println("Tracking Number: " + trackingNumber);
                        System.out.println("Courier Code: " + courierCode);
                    }
                    for (BatchItem batchItem : batchResults.getError()) {
                        String trackingNumber = batchItem.getTrackingNumber();
                        String courierCode = batchItem.getCourierCode();
                        System.out.println("Tracking Number: " + trackingNumber);
                        System.out.println("Courier Code: " + courierCode);
                    }
                }
            } catch (TrackingMoreException e) {
                System.err.println("error：" + e.getMessage());
            } catch (IOException e) {
                System.err.println("error：" + e.getMessage());
            }
        }
    }


Get status of the shipment:

    package com.trackingmore.example.ups;
    
    import com.trackingmore.TrackingMore;
    import com.trackingmore.exception.TrackingMoreException;
    import com.trackingmore.model.TrackingMoreResponse;
    
    import java.io.IOException;
    import java.util.List;
    
    public class GetUPSTrackingExample {
    
        public static void main(String[] args) {
            try {
                String apiKey = "your api key";
                TrackingMore trackingMore = new TrackingMore(apiKey);
                
                GetTrackingResultsParams trackingParams = new GetTrackingResultsParams();
                
                // trackingParams.setTrackingNumbers("1ZC6G8280303220451,1ZR8E2200404839077");
                trackingParams.setCourierCode("ups");
                trackingParams.setCreatedDateMin("2023-08-23T06:00:00+00:00");
                trackingParams.setCreatedDateMax("2023-09-05T07:20:42+00:00");
                TrackingMoreResponse<List<Tracking>> result = trackingMore.trackings.GetTrackingResults(trackingParams);
                System.out.println(result.getMeta().getCode());
                List<Tracking> trackings = result.getData();
                for (Tracking tracking : trackings) {
                    String trackingNumber = tracking.getTrackingNumber();
                    String courierCode = tracking.getCourierCode();
                    
                    System.out.println("Tracking Number: " + trackingNumber);
                    System.out.println("Courier Code: " + courierCode);
                }
            } catch (TrackingMoreException e) {
                System.err.println("error：" + e.getMessage());
            } catch (IOException e) {
                System.err.println("error：" + e.getMessage());
            }
        }
    }


Update a tracking by ID:

    package com.trackingmore.example.ups;
    
    import com.trackingmore.TrackingMore;
    import com.trackingmore.exception.TrackingMoreException;
    import com.trackingmore.model.TrackingMoreResponse;
    
    import java.io.IOException;
    import java.util.List;
    
    public class UpdateUPSTrackingExample {
    
        public static void main(String[] args) {
            try {
                String apiKey = "your api key";
                TrackingMore trackingMore = new TrackingMore(apiKey);
                
                String idString = "9a2f732e29b5ed2071d4cf6b5f4a3d19";
                UpdateTrackingParams updateTrackingParams = new UpdateTrackingParams();
                updateTrackingParams.setCustomerName("New name");
                updateTrackingParams.setNote("New tests order note");
                TrackingMoreResponse<UpdateTracking> result = trackingMore.trackings.UpdateTrackingByID(idString, updateTrackingParams);
                System.out.println(result.getMeta().getCode());
                System.out.println(result.getData());
                if(result.getData() != null){
                    UpdateTracking  updateTracking= result.getData();
                    System.out.println(updateTracking);
                    System.out.println(updateTracking.getTrackingNumber());
                }
            } catch (TrackingMoreException e) {
                System.err.println("error：" + e.getMessage());
            } catch (IOException e) {
                System.err.println("error：" + e.getMessage());
            }
        }
    }
