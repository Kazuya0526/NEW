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
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Quadratic Function Graph</title>
  <script src="https://d3js.org/d3.v7.min.js"></script>
  <style>
    body {
      font-family: Arial, sans-serif;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
      background-color: #f4f4f4;
    }
    svg {
      border: 1px solid black;
    }
  </style>
</head>
<body>

  <div id="chart"></div>

  <script>
    // SVGのサイズ
    const width = 800;
    const height = 400;

    // 二次関数のパラメータ
    const a = 0.01;  // x^2の係数
    const b = -1;    // xの係数
    const c = 0;     // 定数項

    // 二次関数の式
    const quadratic = x => a * x * x + b * x + c;

    // SVGコンテナを作成
    const svg = d3.select("#chart")
                  .append("svg")
                  .attr("width", width)
                  .attr("height", height);

    // 軸の範囲を設定
    const xScale = d3.scaleLinear()
                     .domain([-width / 2, width / 2])
                     .range([0, width]);

    const yScale = d3.scaleLinear()
                     .domain([height / 2, -height / 2])  // 中央を0にして上下反転
                     .range([0, height]);

    // x軸のデータ
    const data = d3.range(-width / 2, width / 2, 1).map(x => {
      return {x: x, y: quadratic(x)};
    });

    // 線を描画
    const line = d3.line()
                   .x(d => xScale(d.x))
                   .y(d => yScale(d.y));

    svg.append("path")
       .data([data])
       .attr("class", "line")
       .attr("d", line)
       .attr("fill", "none")
       .attr("stroke", "blue")
       .attr("stroke-width", 2);

    // X軸を描画
    svg.append("line")
       .attr("x1", xScale(0))
       .attr("y1", 0)
       .attr("x2", xScale(0))
       .attr("y2", height)
       .attr("stroke", "black")
       .attr("stroke-width", 2);

    // Y軸を描画
    svg.append("line")
       .attr("x1", 0)
       .attr("y1", yScale(0))
       .attr("x2", width)
       .attr("y2", yScale(0))
       .attr("stroke", "black")
       .attr("stroke-width", 2);
  </script>

</body>
</html>