Project Title:
TidE-Up

Description:
The aim of the project is to allow citizens to openly view, endorse, or 
oppose Businesses and to provide commonly needed resources that may take 
more time to search online.

Dependencies Used:
react-router
bootstrap(cdn)

API's Used:
Node.js
Railway (Database)

Get Started
Visit Railway.io to get a 30 day free trial 
or
Contact me directly for current Database configs/setup

--Table Schema Below--

-- User table
CREATE TABLE "user" (
  "user_id" SERIAL PRIMARY KEY,
  "email" VARCHAR(255) NOT NULL UNIQUE,
  "password_hash" VARCHAR(255) NOT NULL,
  "username" VARCHAR(50) NOT NULL UNIQUE,
  "first_name" VARCHAR(50),
  "last_name" VARCHAR(50),
  "birth_date" DATE,
  "role" VARCHAR DEFAULT user,
  "created_at" TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  "updated_at" TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  CHECK (email LIKE '%@%.%')
);

-- Industry Type table (Reference)
CREATE TABLE "industry_type" (
  "industry_type_id" SERIAL PRIMARY KEY,
  "name" VARCHAR(100) NOT NULL UNIQUE,
  "description" VARCHAR(500)
);

-- Business table
CREATE TABLE "business" (
  "business_id" SERIAL PRIMARY KEY,
  "name" VARCHAR(100) NOT NULL,
  "mission" VARCHAR(500),
  "city" VARCHAR(100),
  "state" VARCHAR(2),
  "zipcode" VARCHAR(10),
  "price_range" INTEGER CHECK (price_range >= 1 AND price_range <= 5),
  "website_url" VARCHAR(255),
  "founded" DATE,
  "industry_type_id" INTEGER NOT NULL,
  "created_at" TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  "updated_at" TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY ("industry_type_id") REFERENCES "industry_type" ("industry_type_id") ON DELETE CASCADE
);

-- Endorsement Type table (Reference)
CREATE TABLE "endorsement_type" (
  "endorsement_type_id" SERIAL PRIMARY KEY,
  "name" VARCHAR(100) NOT NULL UNIQUE,
  "description" VARCHAR(500)
);

-- Endorsement table
CREATE TABLE "endorsement" (
  "endorsement_id" SERIAL PRIMARY KEY,
  "business_id" INTEGER NOT NULL,
  "user_id" INTEGER NOT NULL,
  "description" VARCHAR(1000),
  "endorsement_type_id" INTEGER NOT NULL,
  "created_at" TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  UNIQUE(business_id, user_id, endorsement_type_id),
  FOREIGN KEY ("business_id") REFERENCES "business" ("business_id") ON DELETE CASCADE,
  FOREIGN KEY ("user_id") REFERENCES "user" ("user_id") ON DELETE CASCADE,
  FOREIGN KEY ("endorsement_type_id") REFERENCES "endorsement_type" ("endorsement_type_id")
);

-- Report Type table (Reference)
CREATE TABLE "report_type" (
  "report_type_id" SERIAL PRIMARY KEY,
  "name" VARCHAR(100) NOT NULL UNIQUE
);

-- Report table
CREATE TABLE "report" (
  "report_id" SERIAL PRIMARY KEY,
  "business_id" INTEGER NOT NULL,
  "user_id" INTEGER NOT NULL,
  "report_type_id" INTEGER NOT NULL,
  "description" VARCHAR(1000),
  "created_at" TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY ("business_id") REFERENCES "business" ("business_id") ON DELETE CASCADE,
  FOREIGN KEY ("user_id") REFERENCES "user" ("user_id") ON DELETE CASCADE,
  FOREIGN KEY ("report_type_id") REFERENCES "report_type" ("report_type_id")
);

-- Review table
CREATE TABLE "review" (
  "review_id" SERIAL PRIMARY KEY,
  "business_id" INTEGER NOT NULL,
  "user_id" INTEGER NOT NULL,
  "description" VARCHAR(1000),
  "rating" INTEGER NOT NULL CHECK (rating >= 1 AND rating <= 5),
  "created_at" TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  "updated_at" TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY ("business_id") REFERENCES "business" ("business_id") ON DELETE CASCADE,
  FOREIGN KEY ("user_id") REFERENCES "user" ("user_id") ON DELETE CASCADE
);

-- ===== SEED DATA =====

-- Insert Industry Types
INSERT INTO industry_type (name, description) VALUES
    ('Agriculture, Forestry & Fishing', 'This industry cultivates and harvests crops, livestock, and seafood.'),
    ('Artificial Intelligence', 'The artificial intelligence industry develops and deploys machine learning, automation, and intelligent systems.'),
    ('Construction', 'The construction industry plans, builds, and maintains structures.'),
    ('E-Commerce & Online Retail', 'E-commerce companies sell products directly to consumers via online platforms.'),
    ('Education', 'The education industry consists of public and private institutions providing academic schooling and training.'),
    ('Entertainment & Leisure', 'The entertainment and leisure industry provides recreational experiences and amusement to consumers.'),
    ('Finance & Insurance', 'The finance and insurance industry manages money and provides financial protection.'),
    ('Government & Public Administration', 'The government sector establishes law and provides essential services.'),
    ('Healthcare', 'The healthcare industry provides vital services and products that prevent, diagnose, and treat illnesses.'),
    ('Information Technology', 'The IT industry develops hardware, software, cybersecurity, and networking solutions.'),
    ('Legal', 'The legal industry provides advice, representation, and dispute mediation services.'),
    ('Lodging', 'The lodging industry provides temporary overnight accommodations to travelers through properties such as hotels, motels, resorts, inns, hostels, and campgrounds.'),
    ('Manufacturing', 'Manufacturers design, produce, and distribute finished goods.'),
    ('Media & Entertainment', 'The media sector informs and entertains through various platforms.'),
    ('Real Estate & Property Management', 'The real estate industry develops, sells, and manages properties.'),
    ('Retail', 'The retail industry consists of businesses selling finished products directly to end consumers for personal use.'),
    ('Telecommunications', 'The telecommunications industry provides voice, data, and video transmission services.'),
    ('Transportation & Logistics', 'The transportation industry moves people and goods via various means.'),
    ('Utilities', 'The utility industry provides essential public services including electric power, natural gas, and water.'),
    ('Wholesale & Distribution', 'Wholesale companies acquire products in bulk to resell to retailers.');

-- Insert Endorsement Types
INSERT INTO endorsement_type (name, description) VALUES
    ('Like', 'Users express approval or positive sentiment toward a business'),
    ('Love', 'Users express strong enthusiasm and emotional connection to a business'),
    ('Recommend', 'Users formally recommend a business to others'),
    ('Award', 'Industry recognition or official award given to a business'),
    ('Certification', 'Official certification or credential earned by a business'),
    ('Partnership', 'Strategic partnership or collaboration endorsement'),
    ('Media Feature', 'Business featured in media publications or outlets'),
    ('Verified Purchase', 'Endorsement from verified customer who purchased');

-- Insert Report Types
INSERT INTO report_type (name) VALUES
    ('Spam'),
    ('Inappropriate Content'),
    ('Fake Business'),
    ('Fraud'),
    ('Misleading Information'),
    ('Copyright Violation'),
    ('Safety Violation'),
    ('False Credentials'),
    ('Service Complaint'),
    ('Scam Alert');



-- Insert Sample Businesses
INSERT INTO business (name, mission, city, state, zipcode, price_range, website_url, founded, industry_type_id) VALUES
    ('Kroger', 'End hunger and waste in our communities across the country', 'Cincinnati', 'OH', '45202', 2, 'https://www.thekrogerco.com/', '1917-01-04', 19),
    ('Apple', 'To make a dent in the universe', 'Cupertino', 'CA', '95014', 4, 'https://www.apple.com/', '1976-04-01', 10),
    ('Amazon', 'To be Earth''s most customer-centric company', 'Seattle', 'WA', '98109', 2, 'https://www.amazon.com/', '1994-07-05', 20),
    ('Tesla', 'To accelerate the world''s transition to sustainable energy', 'Austin', 'TX', '78702', 4, 'https://www.tesla.com/', '2003-07-01', 18),
    ('Google', 'To organize the world''s information and make it universally accessible', 'Mountain View', 'CA', '94043', 3, 'https://www.google.com/', '1998-09-04', 10),
    ('McDonald''s', 'To serve quality food people love', 'Chicago', 'IL', '60601', 1, 'https://www.mcdonalds.com/', '1955-04-15', 1),
    ('Hilton Hotels', 'We hospitality', 'McLean', 'VA', '22102', 3, 'https://www.hiltonworldwide.com/', '1919-01-01', 2),
    ('Netflix', 'To entertain the world', 'Los Gatos', 'CA', '95032', 3, 'https://www.netflix.com/', '1997-08-29', 14);
