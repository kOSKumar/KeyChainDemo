# KeyChainDemo

//
//
//  ViewController.swift
//  KeyChainDemo
//
//  Created by ameet on 18/12/17.
//  Copyright © 2017 amit. All rights reserved.
//
import UIKit

class ViewController: UIViewController {

    @IBOutlet weak var userName: UITextField!
    @IBOutlet weak var password: UITextField!
    
    var passwordItems = [KeychainPasswordItem]()

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
    }
    
    override func viewWillAppear(_ animated: Bool) {
        super.viewWillAppear(animated)
        
        do {
            passwordItems = try KeychainPasswordItem.passwordItems(forService: KeychainConfiguration.serviceName, accessGroup: KeychainConfiguration.accessGroup)
        }
        catch {
            fatalError("Error fetching password items - \(error)")
        }
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
    @IBAction func loginClicked(_ sender: Any) {
        
        guard let newAccountName = userName.text, let newPassword = password.text, !newAccountName.isEmpty && !newPassword.isEmpty else { return }
        
        // Check if we need to update an existing item or create a new one.
        do {
            // Create a keychain item with the original account name.
            let passwordItem = KeychainPasswordItem(service: KeychainConfiguration.serviceName, account: newAccountName, accessGroup: KeychainConfiguration.accessGroup)

            let password = try passwordItem.readPassword()
            if password == newPassword {
                
                print("Login success.")
                self.performSegue(withIdentifier: "home", sender: self)
            }

        } catch {
            let passwordItem = KeychainPasswordItem(service: KeychainConfiguration.serviceName, account: newAccountName, accessGroup: KeychainConfiguration.accessGroup)
            
            // Save the password for the new item.
            do {
                try passwordItem.savePassword(newPassword)
            } catch {
                print("Error in saving user details keychain - \(error)")
            }
            print("No user credentials in Keychain.")
        }
    }
}

----------------------------

//
//  ViewController.swift
//  KeyChainDemo
//
//  Created by ameet on 18/12/17.
//  Copyright © 2017 amit. All rights reserved.
//
import UIKit

class ViewController: UIViewController {

    @IBOutlet weak var userName: UITextField!
    @IBOutlet weak var password: UITextField!
    
    var passwordItems = [KeychainPasswordItem]()

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
    }
    
    override func viewWillAppear(_ animated: Bool) {
        super.viewWillAppear(animated)
        
        do {
            passwordItems = try KeychainPasswordItem.passwordItems(forService: KeychainConfiguration.serviceName, accessGroup: KeychainConfiguration.accessGroup)
        }
        catch {
            fatalError("Error fetching password items - \(error)")
        }
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
    @IBAction func loginClicked(_ sender: Any) {
        
        guard let newAccountName = userName.text, let newPassword = password.text, !newAccountName.isEmpty && !newPassword.isEmpty else { return }
        
        // Check if we need to update an existing item or create a new one.
        do {
            // Create a keychain item with the original account name.
            let passwordItem = KeychainPasswordItem(service: KeychainConfiguration.serviceName, account: newAccountName, accessGroup: KeychainConfiguration.accessGroup)

            let password = try passwordItem.readPassword()
            if password == newPassword {
                
                print("Login success.")
                self.performSegue(withIdentifier: "home", sender: self)
            }

        } catch {
            let passwordItem = KeychainPasswordItem(service: KeychainConfiguration.serviceName, account: newAccountName, accessGroup: KeychainConfiguration.accessGroup)
            
            // Save the password for the new item.
            do {
                try passwordItem.savePassword(newPassword)
            } catch {
                print("Error in saving user details keychain - \(error)")
            }
            print("No user credentials in Keychain.")
        }
    }
}

--------------------------------------------

//
//  SignupViewController.swift
//  KeyChainDemo
//
//  Created by ameet on 18/12/17.
//  Copyright © 2017 amit. All rights reserved.
//

import UIKit

class SignupViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()

        // Do any additional setup after loading the view.
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }

    @IBOutlet weak var userName: UITextField!
    @IBOutlet weak var password: UITextField!
    @IBOutlet weak var saveButton: UIButton!
    
    
    @IBAction func saveAction(_ sender: Any) {
        
        guard let newAccountName = userName.text, let newPassword = password.text, !newAccountName.isEmpty && !newPassword.isEmpty else { return }
        
        // Check if we need to update an existing item or create a new one.
        do {
                // Create a keychain item with the original account name.
                let passwordItem = KeychainPasswordItem(service: KeychainConfiguration.serviceName, account: newAccountName, accessGroup: KeychainConfiguration.accessGroup)
                
                // Update the account name and password.
//                try passwordItem.renameAccount(newAccountName)
//                try passwordItem.savePassword(newPassword)
                _ = try passwordItem.readPassword()

                // This is a new account, create a new keychain item with the account name.

//            }
        }
        catch {
            let passwordItem = KeychainPasswordItem(service: KeychainConfiguration.serviceName, account: newAccountName, accessGroup: KeychainConfiguration.accessGroup)
            
            // Save the password for the new item.
            do {
            try passwordItem.savePassword(newPassword)
            } catch {
                print("Error in saving user details keychain - \(error)")
            }
            print("No user credentials in Keychain.")
            dismiss(animated: true, completion: nil)

        }
    }
    
    func updateSaveButtonState() {
        guard isViewLoaded else { return }
        
        // Enable the save button if both account and password fields contain text.
        if let newAccount = userName.text, let newPassword = password.text, !newAccount.isEmpty && !newPassword.isEmpty {
            saveButton?.isEnabled = true
        }
        else {
            saveButton?.isEnabled = false
        }
    }
    
    @IBAction func textFieldChanged(_ sender: AnyObject) {
        updateSaveButtonState()
    }
}
-----------------------------
