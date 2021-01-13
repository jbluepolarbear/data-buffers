# data_buffers
DataBuffer and DataBitBuffer built on top of ArrayBuffers that grows when needed.

# ArrayBufferProvider
# DataBuffer
# DataBitBuffer

Usage:
    const arrayBufferProvider = new ArrayBufferProvider();
    const dataBuffer = new DataBuffer(arrayBufferProvider);
    const sut = new DataBitBuffer(dataBuffer);
    let i: number;
    let value: number;

    let testValues = [];
    // input
    for (i = 0; i < 7; ++i) {
      value = 1 << i;
      sut.setInt8(value);
      testValues.push(value);
    }

    for (i = 0; i < 7; ++i) {
      value = 1 << (i + 7);
      sut.setInt16(value);
      testValues.push(value);
    }

    for (i = 0; i < 15; ++i) {
      value = 1 << (i + 15 - 1);
      sut.setInt32(value);
      testValues.push(value);
    }

    for (i = 1; i < 15; ++i) {
      value = 1 << (i - 1);
      sut.setBits(i, value);
      testValues.push(value);
    }

    var packed = arrayBufferProvider.arrayBufferToBase64({
      buffer: sut.getArrayBuffer(),
      length: sut.getArrayBufferOffset(),
    });

    var result = arrayBufferProvider.base64ToArrayBuffer(packed);
    sut.loadBuffer(result);
    arrayBufferProvider.giveArrayBufferBack(result.buffer);
    sut.setOffset(0);

    for (i = 0; i < 7; ++i) {
      console.log(sut.getInt8());
      testValues.shift();
    }

    for (i = 0; i < 7; ++i) {
      console.log(sut.getInt16());
      testValues.shift();
    }

    for (i = 0; i < 15; ++i) {
      console.log(sut.getInt32());
      testValues.shift();
    }

    for (i = 1; i < 15; ++i) {
      console.log(sut.getBits(i));
      testValues.shift();
    }