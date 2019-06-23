# task-11-atm-program
import os
import time
def clear_screen():
  
    os.system('clear')
    print()  
def read_file(file_name):
    opened_file = open(file_name, 'r')
    lines_list = []
    for line in opened_file:
        line = line.split()
        lines_list.append(line)
    
    return lines_list

def print_process(process):
    date = '{}'.format(process[2:7])
    print('{0}\t{1}\t{2}{3: ^10} {4: ^10}'.format(
        process[0],
        process[1].center(len('change_password')),
        date.center(len(date)),
        process[7],
        process[8]
    )
    )


def withdraw(ls):


    current_balance = int(ls[3])
  
    print('Your current balance: ' + ls[3])

    withdraw_amount = int(input('Enter withdraw amount: '))

    if withdraw_amount > current_balance:
        print("ERROR: You can't withdraw more than your current balance")
    else:
        current_balance -= abs(withdraw_amount)  

    file_name = ls[0] + '.txt'
    process_list = read_file(file_name)
    id_file = open(file_name, 'a')

    if len(process_list) == 0:
        
        last_id = 1
    else:
        last_id = int(process_list[len(process_list)-1][0]) + 1
  

    id_file.write('{0}\twithdraw\t\t\t{1}\t{2}\t{3}\n'.format(str(last_id), str(time.ctime()), ls[3], str(current_balance)))
    id_file.close()
    ls[3] = str(current_balance)
    print('Your current balance: ' + ls[3])

    return ls


def deposit(ls):

    current_balance = int(ls[3])  
    print('Your current balance: ' + ls[3])
    deposit_amount = int(input('Enter deposit amount: '))


    current_balance += abs(deposit_amount)  
    file_name = ls[0] + '.txt'
    process_list = read_file(file_name)
    id_file = open(file_name, 'a')

    if len(process_list) == 0:  
        last_id = 1
    else:
        last_id = int(process_list[len(process_list) - 1][0]) + 1
        

    id_file.write('{0}\tdeposit\t\t\t\t{1}\t{2}\t{3}\n'.format(str(last_id), str(time.ctime()), ls[3], str(current_balance)))
    id_file.close()
    ls[3] = str(current_balance)
    print('Your current balance: ' + ls[3])

    return ls

def show_history(ls):


    choice = int(input('1) show deposit processes\n2) show withdraw processes\nchoice>> '))

    file_name = ls[0] + '.txt'   
    id_list = read_file(file_name)

   
    top_line = '\nID\t' + 'Type'.center(len('change_password')) + 'Date and Time'.center(40) + 'before'.center(10) + 'after'.center(15)
    print(top_line)
    print('-' * len(top_line))
    if choice == 1:
        for line in id_list:
            if line[1] == 'deposit':
                print_process(line)
    elif choice == 2:
        for line in id_list:
            if line[1] == 'withdraw':
                print_process(line)
    else:
        print('ERROR: Wrong choice')

    input('\nPress Enter to go back..')
    os.system('clear')


def login(acc_list):

    login_id = input('Please, Enter your info(press "Ctrl+C" to go back) \n>>ID: ')
    login_password = input('>>Password: ')
    found = False
    for account in acc_list:
        if account[0] == login_id and account[2] == login_password:
            found = True
            menu2(account)
            break
        else:
            continue

    if not found:
        clear_screen()
        print('Wrong ID or Password')
        login(acc_list)

    else:
        acc_file = open('Accounts.txt', 'w')
        print('Saving changes...')
        
        for acc in acc_list:
            for elements in acc:
                acc_file.write("%s\t" % elements)
            acc_file.write('\n')

            
def create_account(ls):
 
    account_name = input('Enter Your Name (WITHOUT SPACES): ')
    account_password = input('Enter Your Password (WITHOUT SPACES): ')

    print("Creating Your Account .....")
    accounts_file = open('Accounts.txt', 'a')

    if len(ls) == 0:
        new_last_id = 1
    else:
        new_last_id = int(ls[len(ls) - 1][0]) + 1

    line = '{0}\t{1}\t{2}\t0\n'.format(str(new_last_id), account_name, account_password)

    accounts_file.write(line)
    id_file_name = str(new_last_id) + '.txt'
    id_file = open(id_file_name, 'w')

    print("Your Account Has Been Created And Your Id Is " + str(new_last_id))
    id_file.close()
    accounts_file.close()
    ls.append([str(new_last_id), account_name, account_password, '0'])


def menu2(account):

    print("\n---------Hello, {0}--------- ".format(account[1]))
    while True:
        ch = int(input("\n1) show info \n2) show process history\n3) deposit\n4) withdraw\n5) logout\n\nchoice>> "))

        clear_screen()
        if ch == 1:
            print("ID: {}\nName: {}\nBalance: {}\n".format(account[0], account[1], account[3]))
            
        elif ch == 2:
            show_history(account)
        elif ch == 3:
            deposit(account)
        elif ch == 4:
            withdraw(account)
        elif ch == 5:
            break
        else:
            print("ERROR: Wrong choice\n")





accounts_list = read_file('Accounts.txt')
print('>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>WELCOME<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<\n')
count = True
while count:
    choice = int(input('1) Login\n2) Create Account\n3) Exit\n\nchoice>> '))
    if choice == 1:
        clear_screen()
        try:
      
            login(accounts_list)
        except KeyboardInterrupt:
            clear_screen()
    elif choice == 2:
        create_account(accounts_list)
    elif choice == 3:
       
        exit()
    else:
        clear_screen()
        print("ERROR: Wrong choice\n")
