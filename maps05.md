# Maps関連API

## GeoCoder
```java
                LatLng latLng = mMap.getCameraPosition().target;
				// 地図の中心点の緯度・経度を取得
				double lat = latLng.latitude;
				double lon = latLng.longitude;
				// Geocoderで住所を取得
				Geocoder mGeocoder = new Geocoder(this, Locale.getDefault());

				// 住所を格納する文字列バッファ
				StringBuffer sb = new StringBuffer();
				// 緯度、経度、取得可能な最大の住所数を指定
				List<Address> addresses;
				try {
					addresses = mGeocoder.getFromLocation(lat, lon, 1);
					for (Address address : addresses) {
						// 取得された住所情報のサイズでforループ
						int maxSize = address.getMaxAddressLineIndex();
						for (int i = 0; i <= maxSize; i++) {
							sb.append(address.getAddressLine(i));
						}
					}
				} catch (IOException e) {
					// ネットワークエラーの時
					e.printStackTrace();
				} catch (IllegalArgumentException e) {
					// 緯度・経度が範囲外の値の時
					e.printStackTrace();
				}
				Toast.makeText(this,
						"lat:" + lat + " lon:" + lon + " address:" + sb,
						Toast.LENGTH_LONG).show();
```

## 他のAPI

https://developers.google.com/maps/