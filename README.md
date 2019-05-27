package Address;

import Addresses.Address;

import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;
import java.util.Arrays;
import java.util.Optional;
import java.util.function.Supplier;

import com.fasterxml.jackson.databind.ObjectMapper;
import org.json.simple.JSONArray;
import org.json.simple.parser.JSONParser;


public class AddressApplication {

    public static void main(final String[] args) {

        Address address = new Address();
        ObjectMapper mapper = new ObjectMapper();

        try {
          Address[] addresses = mapper.readValue(new File("src/main/resources/addresses.json"), Address[].class);
            Arrays.asList(addresses).forEach(i->System.out.println(prettyPrintAddress(i)));
           prettyPrintAddress(address);
            printallAddressess();
            isValidAddress(address);
            validateAllAddresses();

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    public static String prettyPrintAddress(Address address) {

        //  a. Write a function to return a pretty print version of an address
        // in the format: Type: Line details - city - province/state - postal code – country
        String space = " - ";
        String type = ":";
        return replaceNulls(()->address.getType().getName()).orElse("") + type +
                replaceNulls(()->address.getAddressLineDetail().getLine1()).orElse("") + space +
                replaceNulls(()->address.getAddressLineDetail().getLine2()).orElse("") + space +
                replaceNulls(()->address.getCityOrTown()).orElse("") + space +
                replaceNulls(()->address.getProvinceOrState().getName()).orElse("") + space +
                replaceNulls(()->address.getPostalCode()).orElse("") + space +
                replaceNulls(()->address.getCountry().getName()).orElse("");
    }

    private static <T> Optional<T> replaceNulls(final Supplier<T> resolver) {
        try {
            T result = resolver.get();
            return Optional.ofNullable(result);
        } catch (NullPointerException e) {
            return Optional.empty();
        }
    }

    //   b.  Write a function to pretty print all the addresses in the attached file
    public static  String printallAddressess(){

        JSONParser jsonParser = new JSONParser();

        try  (FileReader addressessReader = new FileReader("src/main/resources/addresses.json"))
           {
                //Read JSON file
                Object obj = jsonParser.parse(addressessReader);
                JSONArray addressess = (JSONArray) obj;

                System.out.println("-----print all the addresses in the attached file----- ");
            //Iterate over address array
               addressess.forEach(listOfAddresses->System.out.println(listOfAddresses));

            } catch (FileNotFoundException e) {
                e.printStackTrace();
            } catch (IOException e) {
                e.printStackTrace();
            } catch (org.json.simple.parser.ParseException e) {
                e.printStackTrace();
            }

         return null;
        }

    private static String isValidAddress(Address address) {

        // d. Write a function to validate an address
        StringBuilder validmessage = new StringBuilder(String.format("Address: %s is valid.", address));
        StringBuilder invalidMessage = new StringBuilder();
        String empty = "";
        Integer postalCode;
        try {
            postalCode = Integer.parseInt(address.getPostalCode());
        } catch (NumberFormatException e) {
            invalidMessage.append("\n postal code is null or not a valid number.");
        }
        if (address.getCountry() == null) {
            invalidMessage.append("\n country is null or not a valid number.");
        }
        if (address.getAddressLineDetail() == null || (address.getAddressLineDetail().getLine1().equals(empty))
                || address.getAddressLineDetail().getLine2().equals(empty)) {
            invalidMessage.append("\n at least one address line is requred.");
        }
        if (address.getCountry() != null && address.getCountry().getCode().equals("ZA")
                && address.getProvinceOrState() == null) {
            invalidMessage.append("\n province is required for country ZA.");
        }

        return (invalidMessage != null )
                ? String.format("Address: %s is invalid.", address) + invalidMessage.toString()
                : validmessage.toString();
    }

    public static void validateAllAddresses() {
        //  e.   For each address in the attached file,
        // determine if it is valid or not, and if not give an
        // indication of what is invalid in a message format of your choice.

        System.out.println("-------------- starting address validation -------------");

        ObjectMapper mapper = new ObjectMapper();

        try {
            Address[] addresses = mapper.readValue(new File("src/main/resources/addresses.json"), Address[].class);

            for (Address address : addresses) {
                String result = isValidAddress(address);
                if (result.contains("invalid")) {
                    System.err.println(result);
                } else {
                    System.out.println(result);
                }
            }
            System.out.println("-------------- end address validation -------------");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

}
