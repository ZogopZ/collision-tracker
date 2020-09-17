# collision-avoidance-app
A collision and avoidance detection system.

# Android Application
- ### Main Thread
    The main thread is responsible for synchronizing the android application. During the initialization the application asks for permissions. The android version used for the development was the Android 7 thus permissions must be asked runtime. Using the ```getMacAddress()``` method the topic at which the android client will be subscribed is initialized. At this point the main activity boots the mqtt client and defines a number of ```OnClickListeners``` to buttons. The application's interface also includes a ```settings``` button that opens a new layout. This layout contains three buttons to solely grant permissions from the user and an inteface for text input. Lastly there is an option to exit the application.
- ### Android Mqtt Client
    The main thread's context is passed as an argument to the ```AndroidMqttClient``` constructor. Even though the client is a non-activity class by using the above method it can produce messages in the application's interface. The constructor of a handler is then called and the MainLooper of the UIThread is passed as an argument. This way the main thread can queue runnables that are about to be executed. The new mqtt client can now connect to the broker and subscribe to the topic that is defined by the mac address of the android terminal. When the subscription is completed a timer starts adding runnables to the queue. One runnable will be added per 2 seconds. This time period value is called "Data Intervals" and can be set in the settings interface of the application. The mqtt client through the context of the main activity and using the method ```getAssets()``` sends random files (from the training set) towards the edge server. Using a ```ByteArrayOutputStream``` and a ```ByteBuffer``` the client sends the name of the file, the mac address of the android terminal itself, the data of the randomly chosen csv file, and the ```FILE``` identifier to the edge server. In case the android mqtt client disconnects from the broker the timer stops and no data is transmitted. Lastly, the client tries to reconnect by calling the ```AndroidMqttClient``` constructor.