# This file is used to install the necessary dependencies for React Native and the app.

# Include the React Native pods script for proper configuration
require_relative '../node_modules/react-native/scripts/react_native_pods.rb'

platform :ios, '18.0'

# Use frameworks if the environment variable USE_FRAMEWORKS is set.
linkage = ENV['USE_FRAMEWORKS']
if linkage != nil
  Pod::UI.puts "Configuring Pod with #{linkage}ally linked Frameworks".green
  use_frameworks! :linkage => linkage.to_sym
end

target 'RecipeboxTemp' do
  # Use native modules and link them
  config = use_native_modules!

  use_react_native!(
    :path => config[:reactNativePath],
    :app_path => "#{Pod::Config.instance.installation_root}/.."
  )

  # Add any additional native dependencies here
  # For example, if you have extra native libraries, they should be added below.
  # pod 'SomeNativeModule', :path => '../node_modules/some-native-module'

  # Test target configuration
  target 'RecipeboxTempTests' do
    inherit! :complete
    # Pods for testing can be added here
  end

  # Post-installation steps for React Native
  post_install do |installer|
    # Ensures that the app works with newer versions of React Native
    react_native_post_install(
      installer,
      config[:reactNativePath],
      :mac_catalyst_enabled => false
    )

    # Clean up Flipper if it's causing build issues (common in some versions of React Native)
    installer.pods_project.targets.each do |target|
      if target.name == 'Flipper'
        target.remove_from_project
      end
    end
  end
end


