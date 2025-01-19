# NEWimport React, { useState } from 'react';
import { View, StyleSheet, Button, Alert } from 'react-native';
import MapboxGL from '@react-native-mapbox-gl/maps';
import Sound from 'react-native-sound';

// Mapboxアクセストークン設定
MapboxGL.setAccessToken('YOUR_MAPBOX_ACCESS_TOKEN'); // ここにMapboxのアクセストークンを設定

const App = () => {
  const [audio, setAudio] = useState(null);

  // 音楽再生の関数
  const playMusic = (region) => {
    let musicUrl = '';
    
    // 地域ごとに音楽のURLを設定
    switch (region) {
      case 'USA':
        musicUrl = 'https://example.com/usa-music.mp3'; // 実際のURLに変更
        break;
      case 'Brazil':
        musicUrl = 'https://example.com/brazil-music.mp3'; // 実際のURLに変更
        break;
      // 他の地域の音楽も追加可能
      default:
        musicUrl = '';
        break;
    }

    if (musicUrl) {
      const sound = new Sound(musicUrl, Sound.MAIN_BUNDLE, (error) => {
        if (error) {
          console.log('Failed to load the sound', error);
          return;
        }
        sound.play();
      });
      setAudio(musicUrl);
      Alert.alert(`Playing music from ${region}`);
    }
  };

  return (
    <View style={{ flex: 1 }}>
      <MapboxGL.MapView style={{ flex: 1 }}>
        <MapboxGL.Camera zoomLevel={2} centerCoordinate={[0, 20]} />
        
        {/* 地域のデータ（GeoJSON） */}
        <MapboxGL.ShapeSource id="region-source" shape={geoJsonData}>
          <MapboxGL.SymbolLayer
            id="region-layer"
            style={styles.symbolLayer}
            onPress={(e) => playMusic(e.payload.name)}
          />
        </MapboxGL.ShapeSource>

        {/* 音楽停止ボタン */}
        {audio && (
          <Button title="Stop Music" onPress={() => setAudio(null)} />
        )}
      </MapboxGL.MapView>
    </View>
  );
};

const geoJsonData = {
  type: 'FeatureCollection',
  features: [
    {
      type: 'Feature',
      properties: { name: 'USA' },
      geometry: {
        type: 'Point',
        coordinates: [-98, 39],
      },
    },
    {
      type: 'Feature',
      properties: { name: 'Brazil' },
      geometry: {
        type: 'Point',
        coordinates: [-55, -10],
      },
    },
    // 他の地域を追加することも可能
  ],
};

const styles = StyleSheet.create({
  symbolLayer: {
    iconImage: '{name}-icon',
    iconSize: 0.2,
  },
});

export default App;
import React, { useEffect } from 'react';
import { View, StyleSheet } from 'react-native';
import MapboxGL from '@react-native-mapbox-gl/maps';

// Mapboxのアクセストークン設定（自分のトークンを入力）
MapboxGL.setAccessToken('YOUR_MAPBOX_ACCESS_TOKEN');

const App = () => {
  useEffect(() => {
    // Mapboxのスタイルが読み込まれることを確認
    MapboxGL.setTelemetryEnabled(false); // ユーザーデータ収集を無効にする（オプション）
  }, []);

  return (
    <View style={{ flex: 1 }}>
      <MapboxGL.MapView style={{ flex: 1 }}>
        {/* 世界地図を表示 */}
        <MapboxGL.Camera
          zoomLevel={1}  // 地図のズームレベル
          centerCoordinate={[0, 20]}  // 地図の初期座標
        />
      </MapboxGL.MapView>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
  },
});

export default App;