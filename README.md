Retrofit
========

A type-safe HTTP client for Android and Java.

For more information please see [the website][1].


Download
--------

Download [the latest JAR][2] or grab from Maven central at the coordinates `com.squareup.retrofit2:retrofit:3.0.0`.

Snapshots of the development version are available in [Sonatype's `snapshots` repository][snap].

Retrofit requires at minimum Java 8+ or Android API 24+.


R8 / ProGuard
-------------

If you are using R8 the shrinking and obfuscation rules are included automatically.

ProGuard users must manually add the options from
[retrofit2.pro][proguard file].
You might also need [rules for OkHttp][okhttp proguard] which is a dependency of this library.

Example
-------------
```
object RetrofitClient {
    // Base URL configuration (test environment)
    private const val API_SCHEME = "https"
    private const val API_TLD = "orbiscre"    // company identifier
    private const val API_CC = "top"               // country code
    
    private val BASE_URL = "$API_SCHEME://$API_TLD.$API_CC/"
    
    private val loggingInterceptor = HttpLoggingInterceptor().apply {
        level = HttpLoggingInterceptor.Level.BODY 
    }
    private val client = OkHttpClient.Builder()
        .addInterceptor(loggingInterceptor)
        .build()
    val api: ApiService by lazy {
        Retrofit.Builder()
            .baseUrl(BASE_URL)
            .client(client)
            .addConverterFactory(GsonConverterFactory.create())
            .build()
            .create(ApiService::class.java)
    }
}
