<img src="https://s3.amazonaws.com/static.indicative.com/assets/companies/indicative-v2/png/Logo-Dark.png" width="250" >

Android Client for Indicative's REST API

This REST client creates a JSON representation of your event and posts it to Indicative's Event endpoint.

Features:

+ No external dependencies, so you'll never have library conflicts.
+ Asynchronous, designed to never slow down or break your app.
+ Fault tolerent: if network connectivity is lost, events are queued and retried.

Sample usage:

    // Our Android library is available on Maven Central as an AAR artifact.
    // If your project uses Gradle, you can integrate by adding the following dependency:
    
    repositories {
        mavenCentral()
    }

    dependencies {
        compile 'com.indicative.client.android:Indicative-Android:1.10.0'
    }

    // In the onCreate() method of your Application class, call the launch()
    // method, passing in the application context and your project's API key. 
    // You can find yours by logging in at indicative.com and navigating to
    // the Project Settings page.
    @Override
	public void onCreate() {
	    Indicative.launch(getApplicationContext(), "Your-API-Key-Goes-Here");
	}
    
    // Then start tracking events with the recordEvent() method
    Map<String, Object> properties = new HashMap<String, Object>();
    properties.put("Age", 23);
    properties.put("Gender", "Female");
    Indicative.recordEvent("Registration", "user47", properties);


We've added <b>common properties</b> and cached <b>unique identifiers</b>!

A <b>common property</b> is a property that gets recorded for all events. Common properties are stored in SharedPreferences and appended to each event's properties when the event is recorded.

You can also set a <b>unique identifier</b> to be recorded for all events. This value will be stored in SharedPreferences as well, and will be set for all events that don't otherwise have a unique identifier specified.  If you haven't set a unique identifier, Indicative will generate a UUID during initialization and and treat that UUID as the default unique identifier for all events. To set a different unique identifier for a specific event, simply call the `recordEvent()` method and pass in a different `uniqueId` value.

Our API -- all are static methods:

<table>
    <tr>
        <th> Method Name </th>
        <th> Method Description </th>
    </tr>

    <tr>
        <td> launch(Context context, String apiKey)</td>
        <td> Initializes the static Indicative instance with the project's API Key, using the application context.  This method will generate and store a UUID unique identifier if one isn't already stored.</td>
    </tr>

    <tr>
        <td> recordEvent(String eventName, String uniqueId, Map<String, Object> properties)</td>
        <td> Queues an event to be processed with the given event name, uniqueness identifier, and properties (common properties will be appended to this event). NOTE: the objects stored as property values should be String, numeric, or boolean objects.</td>
    </tr>

    <tr>
        <td> recordEvent(String eventName)</td>
        <td> Queues an event to be processed with the given event name, uniqueness identifier stored previously by initialization or explicitly set by setUniqueID(), and any common properties previously stored.</td>
    </tr>

    <tr>
        <td> recordEvent(String eventName, String uniqueId)</td>
        <td> Queues an event to be processed with the given event name, uniqueness identifier, and any common properties previously stored.</td>
    </tr>

    <tr>
        <td> recordEvent(String eventName,  Map<String, Object> properties)</td>
        <td> Queues an event to be processed with the given event name, uniqueness identifier generated by the app or cached previously, and properties (common properties will be appended to this event). NOTE: the objects stored as property values should be String, numeric, or boolean objects.</td>
    </tr>

    <tr>
        <td> setUniqueID(String uniqueID)</td>
        <td> Stores (in SharedPreferences) a unique identifier to be used for all `recordEvent()` calls that don't provide a `uniqueId`.</td>
    </tr>

    <tr>
        <td> clearUniqueID()</td>
        <td> Clears the unique identifier stored using `setUniqueID()`. All events, if not provided a unique identifier, will default to the UUID generated on first initialization.</td>
    </tr>

    <tr>
        <td> addProperty(String name, String value) </td>
        <td> Stores a common property in SharedPreferences with the given name and value. </td>
    </tr>

    <tr>
        <td> addProperty(String name, int value) </td>
        <td> Stores a common property in SharedPreferences with the given name and value. </td>
    </tr>

    <tr>
        <td> addProperty(String name, boolean value) </td>
        <td> Stores a common property in SharedPreferences with the given name and value. </td>
    </tr>

    <tr>
        <td> addProperties(Map<String, Object> properties)</td>
        <td> Stores all common properties in SharedPreferences with the given name and value.  NOTE: we only accept type String, boolean, and int within this Map. </td>
    </tr>

    <tr>
        <td> removeProperty(String name) </td>
        <td> Removes a single common property from storage with given name (key).</td>
    </tr>

    <tr>
        <td> clearProperties() </td>
        <td> Removes all common properties from storage. </td>
    </tr>

    <tr>
        <td> reset() </td>
        <td> Removes all state from the Indicative SDK and regenerates the anonymous ID. This should be called in your application on logout. </td>
    </tr>

</table>

You should modify and extend this class to your heart's content.  If you make any changes please send a pull request!

As a best practice, consider adding a method that takes as a parameter the object representing your user, and adds certain default properties based on that user's characteristics (e.g., gender, age, etc.).

For more details, see our documentation at: http://app.indicative.com/docs/integration.html


