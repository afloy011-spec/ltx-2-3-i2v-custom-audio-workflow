# LTX 2 / 2.3 — I2V с кастомным аудио (ComfyUI workflow)

Репозиторий хранит готовый воркфлоу для ComfyUI: **image-to-video** на базе **LTX Video** с отдельной веткой под **кастомное аудио** (энкод/декод LTX Audio VAE, сборка AV latent, превью и сборка видео).

## Файл воркфлоу

| Файл | Описание |
|------|----------|
| [`workflows/LTX-2-3-I2V-Custom-Audio.json`](workflows/LTX-2-3-I2V-Custom-Audio.json) | Экспорт графа ComfyUI (полный пайплайн) |

Импорт: ComfyUI → **Load** (или перетащите JSON в окно).

## Кастомная нода автора: **Afloy Audio Trim**

В этом воркфлоу используется **собственная нода** из пакета **Afloy Audio Tools**:

- **Внутренний тип:** `afloy_TrimAudio`
- **Отображаемое имя в UI:** **Afloy Audio Trim**
- **Категория:** `audio / Afloy Audio Tools`

Нода обрезает входной аудиопоток по времени (`start_sec`, `end_sec`; при `end_sec = -1` — до конца файла) и отдаёт обрезанное аудио (и превью волны, если подключено в вашей версии ноды). Её удобно ставить после **Load Audio**, чтобы длительность и фрагмент точно совпадали с тем, что уходит в LTX audio VAE.

**Без установки custom node `afloy_TrimAudio` этот граф не откроется или ноды будут отсутствовать.** Установите пакет [Afloy Audio Tools](https://github.com/afloy011-spec/comfy-ui) (или вашу копию в `ComfyUI/custom_nodes/`) и перезапустите ComfyUI.

## Прочие зависимости (по составу графа)

Понадобятся стандартные и сторонние ноды ComfyUI, в том числе:

- **LTX Video** (например `LTXVImgToVideoInplace`, `LTXVAudioVAEEncode`, `LTXVConcatAVLatent`, `LTXVSeparateAVLatent`, scheduler / sampling и др.)
- **VHS** — `VHS_VideoCombine` (video combine)
- **rgthree** — `Power Lora Loader (rgthree)`
- Отдельные ноды/модели под **MelBand RoFormer** (лоадер и сэмплер в графе)
- Вспомогательные: KJNodes (`SimpleCalculatorKJ`, `VAELoaderKJ`, `ImageResizeKJv2` и т.д.), утилиты ресайза/маски и subgraph **Get/Set** (ComfyUI)

Точный список типов нод можно посмотреть в JSON. Модели (UNET, VAE, CLIP, LoRA, апскейлер latent и др.) нужно положить в стандартные папки ComfyUI и подставить свои пути в нодах **Load**.

## Лицензия и веса

В репозитории только JSON воркфлоу. Веса моделей сюда не включайте — они в `.gitignore`.
