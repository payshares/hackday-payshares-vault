
desc "Start the dev server"
task :dev do
  exec "bundle exec rerun 'rackup'"
end

desc "Start the pry console"
task :pry do
  exec "bundle exec pry -r ./app.rb"
end

desc "loads the app's environment into memory"
task :environment do
  require './app.rb'
end

namespace :db do
  desc "Migrate the database"
  task :migrate => :environment do
    ActiveRecord::Base.logger = Logger.new(STDOUT)
    ActiveRecord::Migration.verbose = true
    ActiveRecord::Migrator.migrate("db/migrate")
  end
end



desc "Outputs the hex for a random payment"
task :tx => :environment do
  from = Stellar::KeyPair.random
  to = Stellar::KeyPair.random

  tx = Stellar::Transaction.payment({
    account:     from,
    destination: to,
    sequence:    1,
    amount:      [:native, 200 * Stellar::ONE],
  })

  raw = tx.to_xdr
  hex = Stellar::Convert.to_hex raw
  puts hex
end
