import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.util.Calendar;

public class DevelopmentUtils {
    private static final String MD5_ALGORITHM = "md5";
    private static final String OKAY_DEFAULT = "@okjiaoyu.cn";
    private static final String TAG = "DevelopmentUtils";
    private static MessageDigest md5;

    static {
        try {
            md5 = MessageDigest.getInstance("md5");
        } catch (NoSuchAlgorithmException e) {
            e.printStackTrace();
        }
    }

    public static String getEnterDevPassword() {
        String md5Result = encrypt(getVerificationMsg());
        return md5Result.substring(md5Result.length() - 6,
                                   md5Result.length());
    }

    private static String encrypt(String str) {
        return encryptMD5ReturnEvenNumber(encryptMD5ReturnOddNumber(str));
    }

    public static String encryptMD5ReturnOddNumber(String plainText) {
        String result;
        byte[] cipherData = md5.digest(plainText.getBytes());
        StringBuilder oddNumber = new StringBuilder();
        for (byte cipher : cipherData) {
            String toHexStr = Integer.toHexString(cipher & 255);
            if (toHexStr.length() == 1) {
                result = "0" + toHexStr;
            } else {
                result = toHexStr;
            }
            oddNumber.append(result.charAt(0));
        }
        return oddNumber.toString();
    }

    public static String encryptMD5ReturnEvenNumber(String plainText) {
        String result;
        byte[] cipherData = md5.digest(plainText.getBytes());
        StringBuilder oddNumber = new StringBuilder();
        for (byte cipher : cipherData) {
            String toHexStr = Integer.toHexString(cipher & 255);
            if (toHexStr.length() == 1) {
                result = "0" + toHexStr;
            } else {
                result = toHexStr;
            }
            oddNumber.append(result.charAt(1));
        }
        return oddNumber.toString();
    }

    private static String dynamicStr(int month, int day) {
        StringBuilder stringBuilder = new StringBuilder();

        stringBuilder.append(String.valueOf(month)).append("month").append("-").append(String.valueOf(day)).append("day");
        return stringBuilder.toString();
    }

    public static String encryptMD5(String plainText) {
        byte[] cipherData = md5.digest(plainText.getBytes());
        StringBuilder builder = new StringBuilder();
        for (byte cipher : cipherData) {
            String toHexStr = Integer.toHexString(cipher & 255);
            if (toHexStr.length() == 1) {
                toHexStr = "0" + toHexStr;
            }
            builder.append(toHexStr);
        }
        return builder.toString();
    }

    public static String getVerificationMsg() {
        String sequenceNumber = "序列号";
        Calendar c = Calendar.getInstance();
        int year= c.get(1);
        int month = c.get(2) + 1;
        int day = c.get(5);
        StringBuilder stringBuilder = new StringBuilder();


        stringBuilder.append(sequenceNumber).append(String.valueOf(year)).append(String. valueOf(month)).append(String.valueOf(day)).append("@okjiaoyu.cn").append(dynamicStr(month, day));
        return stringBuilder.toString();
    }

    public static void main(String[] args) {
        System.out.print(getEnterDevPassword());
    }
}