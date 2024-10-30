import React, { useEffect, useRef, useState } from "react";
import { View, Text, StyleSheet, Button, Image } from "react-native";
import { useCameraPermissions, CameraView } from "expo-camera";

export default function Camera() {
    const [permissions, requestPermission] = useCameraPermissions();
    const [foto, setFoto] = useState(null);
    const [zoom, setZoom] = useState(0);
    const [facing, setFacing] = useState("back");
    const [flash, setFlash] = useState("off");
    const camera = useRef(null);

    useEffect(() => { requestPermission(); }, []);

    const tirarFoto = async () => {
        if (camera.current) {
            try {
                const img = await camera.current.takePictureAsync();
                setFoto(img);
            } catch (err) {
                console.error(err);
            }
        }
    };

    const trocarZoom = (operacao) => {
        if (operacao === '+' && zoom < 1) {
            setZoom(zoom + 0.1);
        } else if (operacao === '-' && zoom > 0) {
            setZoom(zoom - 0.1);
        }
    };

    const trocarCamera = () => {
        setFacing(facing === "back" ? "front" : "back");
    };

    const alterarFlash = () => {
        setFlash(flash === "off" ? "on" : flash === "on" ? "auto" : "off");
    };

    if (!permissions) {
        return (
            <View>
                <Text>Permissão indefinida</Text>
            </View>
        );
    }

    if (!permissions.granted) {
        return (
            <View>
                <Text>Permissão não autorizada.</Text>
            </View>
        );
    }

    if (foto) {
        return (
            <View style={styles.imagemContainer}>
                <Image style={styles.imagem} source={{ uri: foto.uri }} />
                <Button title="Voltar para a câmera" onPress={() => setFoto(null)} />
            </View>
        );
    }

    return (
        <View style={styles.container}>
            <CameraView 
                zoom={zoom} 
                style={styles.camera} 
                ref={camera} 
                facing={facing} 
                flash={flash}
            >
                <View style={styles.controls}>
                    <View style={styles.flashButton}>
                        <Button title={`Flash: ${flash}`} onPress={alterarFlash} />
                    </View>
                    <View style={styles.zoomButtons}>
                        <Button title="+" onPress={() => trocarZoom('+')} />
                        <Button title="-" onPress={() => trocarZoom('-')} />
                    </View>
                    <View style={styles.captureButton}>
                        <Button title="Tirar Foto" onPress={tirarFoto} />
                    </View>
                    <View style={styles.switchCameraButton}>
                        <Button title="Trocar Câmera" onPress={trocarCamera} />
                    </View>
                </View>
            </CameraView>
        </View>
    );
}

const styles = StyleSheet.create({
    container: {
        flex: 1,
    },
    imagemContainer: {
        flex: 1,
        justifyContent: 'center',
        alignItems: 'center',
    },
    imagem: {
        width: '90%',
        height: '90%',
    },
    camera: {
        flex: 1,
    },
    controls: {
        ...StyleSheet.absoluteFillObject,
    },
    flashButton: {
        position: 'absolute',
        left: 20,
        top: 40,
    },
    zoomButtons: {
        position: 'absolute',
        right: 20,
        top: 40,
        alignItems: 'center',
    },
    captureButton: {
        position: 'absolute',
        bottom: 20,
        alignSelf: 'center',
    },
    switchCameraButton: {
        position: 'absolute',
        bottom: 20,
        right: 20,
    },
});
