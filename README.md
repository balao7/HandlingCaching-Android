# HandlingCaching-Android
Handling Caching In Android


* Clearing cache when any sensitive data is handled inside a page loaded in WebViews:

If the application accesses any sensitive data with a WebView, you may want to use the clearCache() method to delete any files stored locally. Server-side headers like no-cache can also be used to indicate that an application should not cache particular content. the below code snippet explains the steps involved in detail:
		
	Disable the cache :
		
		1. Set the “no-cache” in the headers by using the below code snippet:
	
			HashMap<String,String> noCacheHeaders = new HashMap<String, String>(2);
	        		noCacheHeaders.put("Pragma", "no-cache");
	        		noCacheHeaders.put("Cache-Control", "no-cache");
	                          WebView webView = (WebView) findViewById(R.id.webview);
	        		webView.setWebViewClient(new MyBrowser(), noCacheHeaders);
	                          webView.loadUrl("https://example.com/");


	                          private class MyBrowser extends WebViewClient {
	       		 @Override
	        		public boolean shouldOverrideUrlLoading(WebView view, String url) {
	           		 /* check for the url */
				if (Uri.parse(url).getHost().equals("www.example.com")){
	            			return false;
	       			 }
				else{
				return true;
				}
	   		 }
                    

        2. In the WebViews settings, set the appCache false by using below code snippet:
                          
            WebView webView = (WebView) findViewById(R.id.webview);
			webView.getSettings().setAppCacheEnabled(false);
            webView.getSettings().setAppCacheMaxSize(1);
            webView.getSettings().setCacheMode(WebSettings.LOAD_NO_CACHE);

                        
* Clear the cache: The cache can be cleared at multiple stages in the application using the below code:
			
		webView.clearCache(true);
			
		The cache can be cleared :
			1. just before loading the webpage :

				webView.clearCache(true);
				webView.loadUrl("https://example.com/");


			2. After loading of webpage:

				private class MyBrowser extends WebViewClient {
				        @Override
				        public boolean shouldOverrideUrlLoading(WebView view, String url) {
				            /* check for the url */
				            return false;

				        }

				        @Override
				        public void onPageFinished(WebView view, String url){

				super.onPageFinished(view, url);
				view.clearCache(true);

				        }
				    }
				

		
* The form data such as userId , password, email-Id is saved by default when using webviews. The default behaviour can be overridden by using the below code snippet:
	

		WebView webView = (WebView) findViewById(R.id.webview);
		webView.getSettings().setSaveFormData(false);
		webView.getSettings().setSavePassword(false); // Not needed for API level 18 or greater (deprecated)


		In cases, where there is need to save form data but needs to be cleared at some point, such as on restart of application, the below code snippet can be used:

		WebView webView = (WebView) findViewById(R.id.webview);
		webView.loadUrl(“https://example.com/”)
		webView.clearFormData();


* Disable cookies :

		For API level 21 and above :

		CookieManager cm = CookieManager.getInstance();
		/* to disable all cookies */ 
	       	 cm.setAcceptCookie(false);

		/* to disable thirdparty cookies */
	        	cm.setAcceptThirdPartyCookies(mBrowser, false);
		
		/* to remove all the cookies of the application from the device */
	        	cm.removeAllCookies(mCookieDeleted);

		/* to remove a session specific cookies from the device */
	        	cm.removeSessionCookies(mCookieDeleted);

		/* the callback method */

		private ValueCallback<Boolean> mCookieDeleted = new ValueCallback<Boolean>() {
	        	@Override
	        	public void onReceiveValue(Boolean value) {
	            		Log.d(TAG, "cookies deleted");
	            	// do whatever you want to do after the cookie is deleted; eg : reload tew page etc.
	        }
	    };

	
		For API level below 21:
		
		CookieManager cm = CookieManager.getInstance();
		/* to disable all cookies */ 
	       	 cm.setAcceptCookie(false);
		
		/* to remove all the cookies of the application from the device */
	        	cm.removeAllCookie();

		/* to remove a session specific cookies from the device */
	        	cm.removeSessionCookie();		


Further Reads:

http://www.cis.syr.edu/~wedu/Research/paper/webview_acsac2011.pdf 
http://stackoverflow.com/questions/14392414/android-webviews-clearcache-is-very-slow 
https://groups.google.com/forum/#!topic/android-developers/LiPZ8KxIdkg 
http://stackoverflow.com/questions/13529259/android-webview-cant-disable-cache 

