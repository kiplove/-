# StringBuffer

	 1. StringBuffer继承AbstractStringBuilder并实现了Serializable和CharSequence接口
	 2. AbstractStringBuilder类有两个成员变量：
		byte[] value;  StringBuilder的存放值，区别于String类型使用final修饰，所以StringBuffer是可变的
		int count;     数组的大小
	 3. CharSequence接口是String，StringBuilder和StringBuffer共有的接口，提供序列化字符基本的操作，包括length，charAt，codePoints，toString
	 4. StringBuffer基本每个方法都加synchronized，实现线程安全
	 
    # 代码 
    /**
	 *  缓存，字符串改变都会清零
     */
    private transient String toStringCache;


    static final long serialVersionUID = 3388685877147921107L;

    /**
	 * 接下来是4个构造函数，一个无参，一个以容量为参数，两个以字符类型为参数
     * 
     * 调用AbstractStringBuilder接口，默认初始化value容量为16
     */
    @HotSpotIntrinsicCandidate
    public StringBuffer() {
        super(16);
    }

    @HotSpotIntrinsicCandidate
    public StringBuffer(int capacity) {
        super(capacity);
    }


    @HotSpotIntrinsicCandidate
    public StringBuffer(String str) {
        super(str.length() + 16);
        append(str);
    }

    public StringBuffer(CharSequence seq) {
        this(seq.length() + 16);
        append(seq);
    }

    @Override
    public synchronized int length() {
        return count;
    }

	/**
	 *  value.length
	 */
    @Override
    public synchronized int capacity() {
        return super.capacity();
    }


    @Override
    public synchronized void ensureCapacity(int minimumCapacity) {
        super.ensureCapacity(minimumCapacity);
    }

    /**
     * @since      1.5
     */
    @Override
    public synchronized void trimToSize() {
        super.trimToSize();
    }

    /**
	 * 数值大小设置
     */
    @Override
    public synchronized void setLength(int newLength) {
        toStringCache = null;
        super.setLength(newLength);
    }

    /**
	 * 取值
     */
    @Override
    public synchronized char charAt(int index) {
        return super.charAt(index);
    }


    @Override
    public synchronized int codePointAt(int index) {
        return super.codePointAt(index);
    }


    @Override
    public synchronized int codePointBefore(int index) {
        return super.codePointBefore(index);
    }


    @Override
    public synchronized int codePointCount(int beginIndex, int endIndex) {
        return super.codePointCount(beginIndex, endIndex);
    }


    @Override
    public synchronized int offsetByCodePoints(int index, int codePointOffset) {
        return super.offsetByCodePoints(index, codePointOffset);
    }

    /**
     * 将字符由srcBegin位开始到srcEnd，复制到dst的dstBegin位之后
     */
    @Override
    public synchronized void getChars(int srcBegin, int srcEnd, char[] dst,
                                      int dstBegin)
    {
        super.getChars(srcBegin, srcEnd, dst, dstBegin);
    }

    /**
	 * 给定位置设值
     */
    @Override
    public synchronized void setCharAt(int index, char ch) {
        toStringCache = null;
        super.setCharAt(index, ch);
    }

	/**
	 * 各种append前五个是以不同字符参数作为拼接对象
	 */
    @Override
    public synchronized StringBuffer append(Object obj) {
        toStringCache = null;
        super.append(String.valueOf(obj));
        return this;
    }

    @Override
    @HotSpotIntrinsicCandidate
    public synchronized StringBuffer append(String str) {
        toStringCache = null;
        super.append(str);
        return this;
    }


    public synchronized StringBuffer append(StringBuffer sb) {
        toStringCache = null;
        super.append(sb);
        return this;
    }

    @Override
    synchronized StringBuffer append(AbstractStringBuilder asb) {
        toStringCache = null;
        super.append(asb);
        return this;
    }

    @Override
    public synchronized StringBuffer append(CharSequence s) {
        toStringCache = null;
        super.append(s);
        return this;
    }

	/**
     *  这三个给出是数组或者有范围的数组，所以可以会有超出范围的异常出现
     */
    @Override
    public synchronized StringBuffer append(CharSequence s, int start, int end)
    {
        toStringCache = null;
        super.append(s, start, end);
        return this;
    }

    @Override
    public synchronized StringBuffer append(char[] str) {
        toStringCache = null;
        super.append(str);
        return this;
    }


    @Override
    public synchronized StringBuffer append(char[] str, int offset, int len) {
        toStringCache = null;
        super.append(str, offset, len);
        return this;
    }
	
	/**
	 * 下面六种，将不同的基本类型作为拼接对象
	 */
    @Override
    public synchronized StringBuffer append(boolean b) {
        toStringCache = null;
        super.append(b);
        return this;
    }

    @Override
    @HotSpotIntrinsicCandidate
    public synchronized StringBuffer append(char c) {
        toStringCache = null;
        super.append(c);
        return this;
    }

    @Override
    @HotSpotIntrinsicCandidate
    public synchronized StringBuffer append(int i) {
        toStringCache = null;
        super.append(i);
        return this;
    }

    /**
     * 拼接对象是unicode
     */
    @Override
    public synchronized StringBuffer appendCodePoint(int codePoint) {
        toStringCache = null;
        super.appendCodePoint(codePoint);
        return this;
    }

    @Override
    public synchronized StringBuffer append(long lng) {
        toStringCache = null;
        super.append(lng);
        return this;
    }

    @Override
    public synchronized StringBuffer append(float f) {
        toStringCache = null;
        super.append(f);
        return this;
    }

    @Override
    public synchronized StringBuffer append(double d) {
        toStringCache = null;
        super.append(d);
        return this;
    }

    /**
     *  范围内删除 通过shift字符移位实现，移位用system.arraycopy覆盖实现
     */
    @Override
    public synchronized StringBuffer delete(int start, int end) {
        toStringCache = null;
        super.delete(start, end);
        return this;
    }

    /**
	 * 删除特定字符 通过shift字符移位实现，移位用system.arraycopy覆盖实现
     */
    @Override
    public synchronized StringBuffer deleteCharAt(int index) {
        toStringCache = null;
        super.deleteCharAt(index);
        return this;
    }

    /**
     * 通过shift字符移位实现，移位用system.arraycopy覆盖实现，然后再用getBytes复制str到指定位置
     */
    @Override
    public synchronized StringBuffer replace(int start, int end, String str) {
        toStringCache = null;
        super.replace(start, end, str);
        return this;
    }

    /**
	 * return new String
     */
    @Override
    public synchronized String substring(int start) {
        return substring(start, count);
    }

    @Override
    public synchronized CharSequence subSequence(int start, int end) {
        return super.substring(start, end);
    }

    @Override
    public synchronized String substring(int start, int end) {
        return super.substring(start, end);
    }

    /**
     * 指定位置插入一段字符
     */
    @Override
    public synchronized StringBuffer insert(int index, char[] str, int offset,
                                            int len)
    {
        toStringCache = null;
        super.insert(index, str, offset, len);
        return this;
    }

    @Override
    public synchronized StringBuffer insert(int offset, Object obj) {
        toStringCache = null;
        super.insert(offset, String.valueOf(obj));
        return this;
    }


    @Override
    public synchronized StringBuffer insert(int offset, String str) {
        toStringCache = null;
        super.insert(offset, str);
        return this;
    }


    @Override
    public synchronized StringBuffer insert(int offset, char[] str) {
        toStringCache = null;
        super.insert(offset, str);
        return this;
    }

	/**
	 *  没有使用synchronized的方法都可以使用synchronized方法替代。
	 */
    @Override
    public StringBuffer insert(int dstOffset, CharSequence s) {
        // Note, synchronization achieved via invocations of other StringBuffer methods
        // after narrowing of s to specific type
        // Ditto for toStringCache clearing
        super.insert(dstOffset, s);
        return this;
    }


    @Override
    public synchronized StringBuffer insert(int dstOffset, CharSequence s,
            int start, int end)
    {
        toStringCache = null;
        super.insert(dstOffset, s, start, end);
        return this;
    }


    @Override
    public  StringBuffer insert(int offset, boolean b) {
        // Note, synchronization achieved via invocation of StringBuffer insert(int, String)
        // after conversion of b to String by super class method
        // Ditto for toStringCache clearing
        super.insert(offset, b);
        return this;
    }


    @Override
    public synchronized StringBuffer insert(int offset, char c) {
        toStringCache = null;
        super.insert(offset, c);
        return this;
    }


    @Override
    public StringBuffer insert(int offset, int i) {
        // Note, synchronization achieved via invocation of StringBuffer insert(int, String)
        // after conversion of i to String by super class method
        // Ditto for toStringCache clearing
        super.insert(offset, i);
        return this;
    }


    @Override
    public StringBuffer insert(int offset, long l) {
        // Note, synchronization achieved via invocation of StringBuffer insert(int, String)
        // after conversion of l to String by super class method
        // Ditto for toStringCache clearing
        super.insert(offset, l);
        return this;
    }

    @Override
    public StringBuffer insert(int offset, float f) {
        // Note, synchronization achieved via invocation of StringBuffer insert(int, String)
        // after conversion of f to String by super class method
        // Ditto for toStringCache clearing
        super.insert(offset, f);
        return this;
    }

    @Override
    public StringBuffer insert(int offset, double d) {
        // Note, synchronization achieved via invocation of StringBuffer insert(int, String)
        // after conversion of d to String by super class method
        // Ditto for toStringCache clearing
        super.insert(offset, d);
        return this;
    }


    @Override
    public int indexOf(String str) {
        // Note, synchronization achieved via invocations of other StringBuffer methods
        return super.indexOf(str);
    }


    @Override
    public synchronized int indexOf(String str, int fromIndex) {
        return super.indexOf(str, fromIndex);
    }


    @Override
    public int lastIndexOf(String str) {
        // Note, synchronization achieved via invocations of other StringBuffer methods
        return lastIndexOf(str, count);
    }

    @Override
    public synchronized int lastIndexOf(String str, int fromIndex) {
        return super.lastIndexOf(str, fromIndex);
    }

    @Override
    public synchronized StringBuffer reverse() {
        toStringCache = null;
        super.reverse();
        return this;
    }

    @Override
    @HotSpotIntrinsicCandidate
    public synchronized String toString() {
        if (toStringCache == null) {
            return toStringCache =
                    isLatin1() ? StringLatin1.newString(value, 0, count)
                               : StringUTF16.newString(value, 0, count);
        }
        return new String(toStringCache);
    }

    /**
     * Serializable fields for StringBuffer.
     *
     * @serialField value  char[]
     *              The backing character array of this StringBuffer.
     * @serialField count int
     *              The number of characters in this StringBuffer.
     * @serialField shared  boolean
     *              A flag indicating whether the backing array is shared.
     *              The value is ignored upon deserialization.
     */
    private static final java.io.ObjectStreamField[] serialPersistentFields =
    {
        new java.io.ObjectStreamField("value", char[].class),
        new java.io.ObjectStreamField("count", Integer.TYPE),
        new java.io.ObjectStreamField("shared", Boolean.TYPE),
    };

    /**
     * readObject is called to restore the state of the StringBuffer from
     * a stream.
     */
    private synchronized void writeObject(java.io.ObjectOutputStream s)
        throws java.io.IOException {
        java.io.ObjectOutputStream.PutField fields = s.putFields();
        char[] val = new char[capacity()];
        if (isLatin1()) {
            StringLatin1.getChars(value, 0, count, val, 0);
        } else {
            StringUTF16.getChars(value, 0, count, val, 0);
        }
        fields.put("value", val);
        fields.put("count", count);
        fields.put("shared", false);
        s.writeFields();
    }

    /**
     * readObject is called to restore the state of the StringBuffer from
     * a stream.
     */
    private void readObject(java.io.ObjectInputStream s)
        throws java.io.IOException, ClassNotFoundException {
        java.io.ObjectInputStream.GetField fields = s.readFields();
        char[] val = (char[])fields.get("value", null);
        initBytes(val, 0, val.length);
        count = fields.get("count", 0);
    }

    synchronized void getBytes(byte dst[], int dstBegin, byte coder) {
        super.getBytes(dst, dstBegin, coder);
    }

