import csv
import calendar
import time


def get_hostname():

    hostname_correct = False
    while hostname_correct is False:
        hostname = input("\nPlease complete the hostname, without the countrycode (.de, .com) at the end: https://")
        print(f'\nThanks, the hostname is https://{hostname}."countrycode"\n')
        confirm = input("Is this correct? If yes, press 'y', if no, press 'n': ").upper()
        if confirm == 'Y':
            return hostname


def prepare_url_tuples_list(hostname):

    url_tuples_list = []
    extensions_and_hashes = '.jpg', '.png', '.jpeg', '.gif', '.mp4', '.pdf', '#', '#/', '.js'

    screaming_csv_can_be_opened = False
    while screaming_csv_can_be_opened is False:
        screaming_csv = input('\nWhich Frog CSV file should be processed? Enter the filename without .csv at the end: ')+'.csv'
        try:
            with open(screaming_csv, 'r', encoding='utf8') as csvfile:
                reader = csv.DictReader(csvfile)
                for row in reader:
                    new_dictionary = {key: row['Source'] for key in ['Source']}
                    new_dictionary_b = {key: row['Destination'] for key in ['Target']}
                    new_dictionary.update(new_dictionary_b)
                    row_source = new_dictionary['Source']
                    row_target = new_dictionary['Target']
                    if row_source == row_target:
                        pass
                    elif row_source.endswith(extensions_and_hashes) or row_target.endswith(extensions_and_hashes):
                        pass
                    elif hostname not in row_source or hostname not in row_target:
                        pass
                    elif '/wp-content/' in row_source or '/wp-content/' in row_target:
                        pass
                    elif '/wp-includes/' in row_source or '/wp-includes/' in row_target:
                        pass
                    elif '/page/' in row_source or '/page/' in row_target:
                        pass
                    elif '?page=1' in row_source or '?page=1' in row_target:
                        pass
                    else:
                        current_url_tuple = (row_source, row_target)
                        url_tuples_list.append(current_url_tuple)

                set_conversion_to_remove_duplicates = set(url_tuples_list)
                url_tuples_list = list(set_conversion_to_remove_duplicates)

                screaming_csv_can_be_opened = True
                return url_tuples_list

        except:
            print("\nI couldn't find this file, please try again.")


def get_timestamp():

    current_gmt = time.gmtime()
    time_stamp = calendar.timegm(current_gmt)
    string_stamp = str(time_stamp)

    return string_stamp


def remove_user_selected_urls(url_tuples_list, host, timestamp):

    url_list = []
    for x, y in url_tuples_list:
        url_list.append(x)
        url_list.append(y)
    set_conversion_to_remove_duplicates = set(url_list)
    url_list_for_user_check = sorted(list(set_conversion_to_remove_duplicates))

    with open('URL List ' + host + ' ' + timestamp + '.txt', 'w', encoding='utf8') as f:
        for line in url_list_for_user_check:
            f.write(f"{line}\n")

    start_filtering = False
    print(f'\nHave a look at the file "URL List {host} {timestamp}.txt" and remove all irrelevant URLs, then save the file.')
    while start_filtering is False:
        user_gives_go_ahead = \
            input("\nWhen you are done and have saved and closed the textfile, type DONE and hit enter: ").upper()
        if user_gives_go_ahead == "DONE":
            start_filtering = True

    with open('URL List ' + host + ' ' + timestamp + '.txt', 'r', encoding='utf8') as f:
        user_filtered_url_list = []
        for line in f:
            user_filtered_url_list.append(line.rstrip())
        user_filtered_url_tuple = tuple(user_filtered_url_list)
        filtered_url_list = []
        for x, y in url_tuples_list:
            if x in user_filtered_url_tuple and y in user_filtered_url_tuple:
                stripped_x = str(x).removeprefix(f"https://{hostname}")
                stripped_y = str(y).removeprefix(f"https://{hostname}")
                completed_line = (stripped_x, stripped_y)
                filtered_url_list.append(completed_line)

    filtered_url_list.sort()

    return filtered_url_list


def create_gephi_csv(filtered_url_list, hostname, timestamp):

    header = ['Source', 'Target']
    with open(f'Gephi CSV {hostname} {timestamp}.csv', 'w', encoding='utf8', newline='') as f:
        writer = csv.writer(f)
        writer.writerow(header)
        for line in filtered_url_list:
            writer.writerow(line)

    input(f'\nSuccess! The file "Gephi CSV {hostname} {timestamp}.csv" has been created.'
          f'\n\nYou can hit any key to exit the program. ')

    
if __name__ == '__main__':

    hostname = get_hostname()
    url_tuples_list = prepare_url_tuples_list(hostname)
    timestamp = get_timestamp()
    filtered_url_list = remove_user_selected_urls(url_tuples_list, hostname, timestamp)
    create_gephi_csv(filtered_url_list, hostname, timestamp)
