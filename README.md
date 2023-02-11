#  ElementaryOS WebApp Media Controls

Elementary OS 7 [supports Web Apps out of the box](https://blog.elementary.io/os-7-available-now/#web-apps).
Now it is possible to create Web App for YouTube or YouTube Music, Spotify or any other streaming.
Goal of this project is to connect Web Apps with [wingpanel-indicator-sound](https://github.com/elementary/wingpanel-indicator-sound),
like Music or any other media app is connected, sound indicator has a popup with playback controls
which allow to switch ⏮️ previous or ⏭️ next track and ⏯️ pause/continue playback.

![wingpanel-indicator-sound](https://github.com/elementary/wingpanel-indicator-sound/raw/master/data/screenshot.png?raw=true).

## 💡 Idea

Lets suppose that any application may have its own playback controls on sound indicator popup.
If it is true, some of Web Apps may have these controls.

When Web App is presented on sound indicator's popup, it may react on playback controls,
by sending keyboard events to Web App window (⏮️ Ctrl+P, ⏯️ Ctrl+N, ⏯️ Space) or
by calling [video/audio API](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Client-side_web_APIs/Video_and_audio_APIs).

### Keyboard Event Simulation

```js
function simulateKeyPress(code) {
  const options = {
    /**
     * @see https://developer.mozilla.org/en-US/docs/Web/API/Event/Event#bubbles
     */
    bubbles: true,

    /**
     * @see https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/KeyboardEvent#code
     */
    code,

    /**
     * @see https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/KeyboardEvent#keyCode
     */
    keyCode: code,
  };
  const keydown = new KeyboardEvent('keydown', options);
  const keyup = new KeyboardEvent('keyup', options);
  const target = window.document.getElementsByTagName('body').item(0);

  target.dispatchEvent(keydown);
  setTimeout(() => target.dispatchEvent(keyup), 0);
}

function simulateSpacePress() {
  simulateKeyPress(32);
}
```
