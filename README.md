
import React, { useState, useEffect } from 'react';
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Select, SelectContent, SelectItem, SelectTrigger, SelectValue } from "@/components/ui/select";
import { Calendar } from "@/components/ui/calendar";
import { Popover, PopoverContent, PopoverTrigger } from "@/components/ui/popover";
import { Card, CardContent } from "@/components/ui/card";
import { 
  Menubar,
  MenubarContent,
  MenubarItem,
  MenubarMenu,
  MenubarTrigger,
} from "@/components/ui/menubar";
import { Search, Settings, User, Calendar as CalendarIcon, BarChart3, CreditCard } from 'lucide-react';
import { TopCompaniesCarousel } from '@/components/features/TopCompaniesCarousel';
import { AIResumeMatcher } from '@/components/features/AIResumeMatcher';
import { ChatBot } from '@/components/features/ChatBot';
import { DarkModeToggle } from '@/components/features/DarkModeToggle';
import { VoiceSearch } from '@/components/features/VoiceSearch';
import { JobModeToggle } from '@/components/features/JobModeToggle';
import { SkillGapAnalysis } from '@/components/features/SkillGapAnalysis';
import { AdminDashboard } from '@/components/features/AdminDashboard';
import { LanguageSelector } from '@/components/features/LanguageSelector';
import { ThreeDotMenu } from '@/components/features/ThreeDotMenu';
import { SubscriptionPlans } from '@/components/features/SubscriptionPlans';
import { SupportOptions } from '@/components/features/SupportOptions';
import { QuickPaymentOptions } from '@/components/payment/QuickPaymentOptions';
import { PaymentHistory } from '@/components/payment/PaymentHistory';
import { SubscriptionStatus } from '@/components/payment/SubscriptionStatus';

// Job Assistant API Configuration
const JOB_ASSISTANT_API_KEY = 'AIzaSyAtP7K-YwgojBXuQhtcJlCclynqGy5AyJg';

const Index = () => {
  const [searchQuery, setSearchQuery] = useState('');
  const [jobs, setJobs] = useState([]);
  const [allCategories, setAllCategories] = useState([
    'Technology', 'Healthcare', 'Finance', 'Education', 'Marketing', 'Sales', 
    'Engineering', 'Design', 'Manufacturing', 'Transportation', 'Hospitality', 
    'Retail', 'Construction', 'Agriculture', 'Security', 'Cleaning Services', 
    'Beauty & Wellness', 'Food Service', 'Delivery & Logistics', 'Domestic Help',
    'Skilled Trades', 'Government', 'Non-Profit', 'Entertainment', 'Sports'
  ]);
  const [selectedCategory, setSelectedCategory] = useState('');
  const [showFilters, setShowFilters] = useState(false);
  const [selectedState, setSelectedState] = useState('');
  const [selectedJobProfile, setSelectedJobProfile] = useState('');
  const [salaryFrom, setSalaryFrom] = useState('');
  const [salaryTo, setSalaryTo] = useState('');
  const [selectedDate, setSelectedDate] = useState<Date | undefined>(undefined);

  // Add new state for additional features
  const [selectedJobModes, setSelectedJobModes] = useState(['fulltime']);
  const [userSkills, setUserSkills] = useState(['React', 'JavaScript', 'HTML', 'CSS']);
  const [showAdminDashboard, setShowAdminDashboard] = useState(false);
  const [isGlobalSearch, setIsGlobalSearch] = useState(false);
  const [selectedLanguage, setSelectedLanguage] = useState('English');
  const [showPaymentSection, setShowPaymentSection] = useState(false);

  const states = [
    "Andhra Pradesh", "Arunachal Pradesh", "Assam", "Bihar", "Chhattisgarh",
    "Goa", "Gujarat", "Haryana", "Himachal Pradesh", "Jharkhand", "Karnataka",
    "Kerala", "Madhya Pradesh", "Maharashtra", "Manipur", "Meghalaya", "Mizoram",
    "Nagaland", "Odisha", "Punjab", "Rajasthan", "Sikkim", "Tamil Nadu",
    "Telangana", "Tripura", "Uttar Pradesh", "Uttarakhand", "West Bengal"
  ];

  const jobProfiles = [
    // Technology
    "Software Engineer", "Data Scientist", "Web Developer", "Mobile App Developer",
    // Healthcare
    "Doctor", "Nurse", "Pharmacist", "Physiotherapist", "Medical Assistant",
    // Finance
    "Financial Analyst", "Accountant", "Bank Teller", "Insurance Agent",
    // Education
    "Teacher", "Professor", "Tutor", "Principal", "School Administrator",
    // Marketing & Sales
    "Marketing Manager", "Sales Representative", "Digital Marketer", "Sales Executive",
    // Transportation
    "Driver", "Delivery Boy", "Cab Driver", "Truck Driver", "Auto Rickshaw Driver",
    // Hospitality & Food Service
    "Chef", "Waiter", "Hotel Manager", "Receptionist", "Room Service", "Cook",
    // Retail
    "Shop Keeper", "Cashier", "Store Manager", "Sales Associate",
    // Skilled Trades
    "Electrician", "Plumber", "Carpenter", "Painter", "Mason", "Welder", "Tailor",
    // Cleaning & Maintenance
    "Cleaner", "Housekeeping", "Janitor", "Gardener", "Security Guard",
    // Beauty & Wellness
    "Hair Stylist", "Beautician", "Massage Therapist", "Gym Trainer",
    // Construction
    "Construction Worker", "Site Supervisor", "Civil Engineer", "Architect",
    // Agriculture
    "Farmer", "Agricultural Worker", "Veterinarian", "Farm Manager",
    // Domestic Help
    "Maid", "Baby Sitter", "Elder Care", "House Helper", "Nanny",
    // Others
    "Mechanic", "Watchman", "Peon", "Office Boy", "Delivery Executive"
  ];

  const allJobs = [
    {
      id: 1,
      title: "Software Developer",
      company: "Tech Solutions Pvt Ltd",
      location: "Bangalore",
      state: "Karnataka",
      district: "Bangalore Urban",
      vacancy: 5,
      salary: "₹50,000 - ₹80,000",
      experience: "2-4 years",
      type: "Full Time",
      category: "Technology",
      postedDate: new Date('2024-01-15'),
      description: "Looking for experienced React developers",
      requiredSkills: ['React', 'JavaScript', 'Node.js', 'MongoDB'],
      isGlobal: false
    },
    {
      id: 2,
      title: "Data Scientist",
      company: "Analytics Corp",
      location: "Mumbai",
      state: "Maharashtra",
      district: "Mumbai Suburban",
      vacancy: 3,
      salary: "₹70,000 - ₹1,20,000",
      experience: "3-5 years",
      type: "Full Time",
      category: "Technology",
      postedDate: new Date('2024-01-10'),
      description: "Analyze large datasets and build predictive models",
      requiredSkills: ['Python', 'Machine Learning', 'SQL', 'Data Visualization'],
      isGlobal: false
    },
    {
      id: 3,
      title: "Driver",
      company: "City Cab Services",
      location: "Delhi",
      state: "Delhi",
      district: "Central Delhi",
      vacancy: 10,
      salary: "₹25,000 - ₹35,000",
      experience: "2-5 years",
      type: "Full Time",
      category: "Transportation",
      postedDate: new Date('2024-01-12'),
      description: "Need experienced drivers with valid license",
      requiredSkills: ['Driving License', 'Good Communication', 'Navigation Skills'],
      isGlobal: false
    },
    {
      id: 4,
      title: "Delivery Boy",
      company: "Quick Delivery Co",
      location: "Pune",
      state: "Maharashtra",
      district: "Pune",
      vacancy: 15,
      salary: "₹18,000 - ₹25,000",
      experience: "0-2 years",
      type: "Full Time",
      category: "Delivery & Logistics",
      postedDate: new Date('2024-01-14'),
      description: "Food and package delivery in city areas",
      requiredSkills: ['Two Wheeler License', 'Local Area Knowledge', 'Physical Fitness'],
      isGlobal: false
    },
    {
      id: 5,
      title: "Cleaner",
      company: "Clean Home Services",
      location: "Chennai",
      state: "Tamil Nadu",
      district: "Chennai",
      vacancy: 20,
      salary: "₹15,000 - ₹22,000",
      experience: "0-1 years",
      type: "Full Time",
      category: "Cleaning Services",
      postedDate: new Date('2024-01-11'),
      description: "House and office cleaning services",
      requiredSkills: ['Cleaning Experience', 'Reliability', 'Time Management'],
      isGlobal: false
    },
    {
      id: 6,
      title: "Housekeeping",
      company: "Hotel Grand Palace",
      location: "Goa",
      state: "Goa",
      district: "North Goa",
      vacancy: 8,
      salary: "₹18,000 - ₹25,000",
      experience: "1-3 years",
      type: "Full Time",
      category: "Hospitality",
      postedDate: new Date('2024-01-09'),
      description: "Hotel room cleaning and maintenance",
      requiredSkills: ['Hotel Experience', 'Attention to Detail', 'Team Work'],
      isGlobal: false
    },
    {
      id: 7,
      title: "Tailor",
      company: "Fashion Designer Studio",
      location: "Jaipur",
      state: "Rajasthan",
      district: "Jaipur",
      vacancy: 5,
      salary: "₹20,000 - ₹35,000",
      experience: "2-5 years",
      type: "Full Time",
      category: "Skilled Trades",
      postedDate: new Date('2024-01-13'),
      description: "Experienced tailor for custom clothing",
      requiredSkills: ['Sewing', 'Pattern Making', 'Alterations', 'Fashion Design'],
      isGlobal: false
    },
    {
      id: 8,
      title: "Electrician",
      company: "Power Solutions Ltd",
      location: "Hyderabad",
      state: "Telangana",
      district: "Hyderabad",
      vacancy: 6,
      salary: "₹25,000 - ₹40,000",
      experience: "3-7 years",
      type: "Full Time",
      category: "Skilled Trades",
      postedDate: new Date('2024-01-08'),
      description: "Residential and commercial electrical work",
      requiredSkills: ['Electrical Wiring', 'Safety Protocols', 'Problem Solving'],
      isGlobal: false
    },
    {
      id: 9,
      title: "Nurse",
      company: "City Hospital",
      location: "Kolkata",
      state: "West Bengal",
      district: "Kolkata",
      vacancy: 12,
      salary: "₹30,000 - ₹45,000",
      experience: "1-4 years",
      type: "Full Time",
      category: "Healthcare",
      postedDate: new Date('2024-01-07'),
      description: "Patient care and medical assistance",
      requiredSkills: ['Nursing Degree', 'Patient Care', 'Medical Knowledge'],
      isGlobal: false
    },
    {
      id: 10,
      title: "Cook",
      company: "Tasty Kitchen",
      location: "Ahmedabad",
      state: "Gujarat",
      district: "Ahmedabad",
      vacancy: 4,
      salary: "₹20,000 - ₹30,000",
      experience: "2-5 years",
      type: "Full Time",
      category: "Food Service",
      postedDate: new Date('2024-01-06'),
      description: "Indian and continental cooking",
      requiredSkills: ['Cooking Experience', 'Food Safety', 'Menu Planning'],
      isGlobal: false
    },
    {
      id: 11,
      title: "Security Guard",
      company: "Safe Guard Security",
      location: "Lucknow",
      state: "Uttar Pradesh",
      district: "Lucknow",
      vacancy: 8,
      salary: "₹18,000 - ₹25,000",
      experience: "1-3 years",
      type: "Full Time",
      category: "Security",
      postedDate: new Date('2024-01-05'),
      description: "Building and premises security",
      requiredSkills: ['Security Training', 'Alertness', 'Physical Fitness'],
      isGlobal: false
    },
    {
      id: 12,
      title: "Hair Stylist",
      company: "Glamour Salon",
      location: "Chandigarh",
      state: "Punjab",
      district: "Chandigarh",
      vacancy: 3,
      salary: "₹22,000 - ₹35,000",
      experience: "2-4 years",
      type: "Full Time",
      category: "Beauty & Wellness",
      postedDate: new Date('2024-01-04'),
      description: "Hair cutting, styling and coloring",
      requiredSkills: ['Hair Styling', 'Customer Service', 'Creativity'],
      isGlobal: false
    },
    {
      id: 13,
      title: "Construction Worker",
      company: "Build Well Constructions",
      location: "Indore",
      state: "Madhya Pradesh",
      district: "Indore",
      vacancy: 25,
      salary: "₹15,000 - ₹25,000",
      experience: "0-3 years",
      type: "Full Time",
      category: "Construction",
      postedDate: new Date('2024-01-03'),
      description: "Building construction and labor work",
      requiredSkills: ['Physical Strength', 'Construction Tools', 'Team Work'],
      isGlobal: false
    },
    {
      id: 14,
      title: "Maid",
      company: "Home Care Services",
      location: "Bhopal",
      state: "Madhya Pradesh",
      district: "Bhopal",
      vacancy: 15,
      salary: "₹12,000 - ₹20,000",
      experience: "0-2 years",
      type: "Part Time",
      category: "Domestic Help",
      postedDate: new Date('2024-01-02'),
      description: "House cleaning and maintenance",
      requiredSkills: ['Cleaning', 'Cooking', 'Reliability'],
      isGlobal: false
    },
    {
      id: 15,
      title: "Shop Keeper",
      company: "Local Grocery Store",
      location: "Patna",
      state: "Bihar",
      district: "Patna",
      vacancy: 2,
      salary: "₹18,000 - ₹28,000",
      experience: "1-3 years",
      type: "Full Time",
      category: "Retail",
      postedDate: new Date('2024-01-01'),
      description: "Managing store operations and customer service",
      requiredSkills: ['Customer Service', 'Cash Handling', 'Inventory Management'],
      isGlobal: false
    },
    {
      id: 16,
      title: "React Developer - Freelance",
      company: "Freelance Project",
      location: "Remote",
      state: "Remote",
      district: "Global",
      vacancy: 1,
      salary: "₹2,000 - ₹5,000 per project",
      experience: "1-3 years",
      type: "Freelance",
      category: "Technology",
      postedDate: new Date('2024-01-10'),
      description: "Build modern web applications using React",
      requiredSkills: ['React', 'JavaScript', 'TypeScript', 'Node.js'],
      isGlobal: true
    }
  ];

  useEffect(() => {
    setJobs(allJobs);
  }, []);

  const handleLanguageChange = (language: string) => {
    setSelectedLanguage(language);
    console.log('Language changed to:', language);
    // Add language change logic here
  };

  const handleVoiceQuery = (query: string) => {
    console.log('Voice query received:', query);
    setSearchQuery(query);
    handleSearch();
  };

  const handleJobModeChange = (modes: string[]) => {
    setSelectedJobModes(modes);
    console.log('Job modes changed:', modes);
  };

  const handleSkillUpdate = (skills: string[]) => {
    setUserSkills(skills);
    console.log('Skills updated:', skills);
  };

  const handleSearch = () => {
    console.log("Search button clicked with query:", searchQuery);
    console.log("Job Assistant API Key configured:", JOB_ASSISTANT_API_KEY ? 'Yes' : 'No');
    console.log("Filters:", { selectedState, selectedJobProfile, salaryFrom, salaryTo, selectedJobModes, isGlobalSearch });
    
    let filteredJobs = [...allJobs];
    
    // Filter by job modes
    if (selectedJobModes.length > 0) {
      filteredJobs = filteredJobs.filter(job => {
        if (selectedJobModes.includes('fulltime') && job.type === 'Full Time') return true;
        if (selectedJobModes.includes('freelance') && job.type === 'Freelance') return true;
        if (selectedJobModes.includes('internship') && job.type === 'Internship') return true;
        if (selectedJobModes.includes('global') && job.isGlobal) return true;
        return false;
      });
    }

    // Filter by global opportunities
    if (isGlobalSearch) {
      filteredJobs = filteredJobs.filter(job => job.isGlobal);
    }

    if (searchQuery) {
      filteredJobs = filteredJobs.filter(job =>
        job.title.toLowerCase().includes(searchQuery.toLowerCase()) ||
        job.company.toLowerCase().includes(searchQuery.toLowerCase()) ||
        job.description.toLowerCase().includes(searchQuery.toLowerCase())
      );
    }

    if (selectedState) {
      filteredJobs = filteredJobs.filter(job => job.state === selectedState);
    }

    if (selectedJobProfile) {
      filteredJobs = filteredJobs.filter(job => job.title === selectedJobProfile);
    }

    if (salaryFrom) {
      filteredJobs = filteredJobs.filter(job => parseInt(job.salary.split('₹')[1].split(' - ')[0].replace(',', '')) >= parseInt(salaryFrom));
    }

    if (salaryTo) {
      filteredJobs = filteredJobs.filter(job => parseInt(job.salary.split('₹')[1].split(' - ')[1].replace(',', '')) <= parseInt(salaryTo));
    }

    if (selectedCategory) {
      filteredJobs = filteredJobs.filter(job => job.category === selectedCategory);
    }

    if (selectedDate) {
      filteredJobs = filteredJobs.filter(job => job.postedDate <= selectedDate);
    }
    
    setJobs(filteredJobs);
    setShowFilters(false);
  };

  return (
    <div className="min-h-screen bg-gradient-to-br from-blue-50 via-white to-cyan-50 animate-fade-in">
      {/* Animated background elements */}
      <div className="fixed inset-0 overflow-hidden pointer-events-none">
        <div className="absolute top-10 left-10 w-72 h-72 bg-blue-200 rounded-full mix-blend-multiply filter blur-xl opacity-70 animate-blob"></div>
        <div className="absolute top-10 right-10 w-72 h-72 bg-cyan-200 rounded-full mix-blend-multiply filter blur-xl opacity-70 animate-blob animation-delay-2000"></div>
        <div className="absolute -bottom-8 left-20 w-72 h-72 bg-purple-200 rounded-full mix-blend-multiply filter blur-xl opacity-70 animate-blob animation-delay-4000"></div>
      </div>

      <nav className="w-full bg-gradient-to-r from-blue-600 to-cyan-600 shadow-lg relative z-10">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
          <div className="flex justify-between items-center h-16">
            <div className="flex items-center space-x-3">
              <h1 className="text-2xl font-bold text-white animate-pulse">BHAGYA</h1>
              <div className="flex space-x-1">
                <div className="w-2 h-2 bg-green-400 rounded-full animate-ping"></div>
                <div className="w-2 h-2 bg-yellow-400 rounded-full animate-ping animation-delay-1000"></div>
                <div className="w-2 h-2 bg-red-400 rounded-full animate-ping animation-delay-2000"></div>
              </div>
            </div>
            
            <div className="flex items-center space-x-4">
              <LanguageSelector onLanguageChange={handleLanguageChange} />
              <ThreeDotMenu />
              <SubscriptionPlans />
              <SupportOptions />
              <DarkModeToggle />
              
              {/* Payment Menu */}
              <Menubar className="border-none bg-transparent">
                <MenubarMenu>
                  <MenubarTrigger className="text-white hover:bg-white/20 border-none bg-transparent focus:bg-white/20 data-[state=open]:bg-white/20 transition-all duration-300">
                    <CreditCard className="w-4 h-4 mr-2" />
                    Payments
                  </MenubarTrigger>
                  <MenubarContent>
                    <MenubarItem 
                      className="font-bold text-gray-800 cursor-pointer hover:bg-blue-50 transition-colors"
                      onClick={() => setShowPaymentSection(!showPaymentSection)}
                    >
                      Payment Options
                    </MenubarItem>
                    <MenubarItem className="font-bold text-gray-800 cursor-pointer hover:bg-blue-50 transition-colors">
                      Payment History
                    </MenubarItem>
                    <MenubarItem className="font-bold text-gray-800 cursor-pointer hover:bg-blue-50 transition-colors">
                      Subscription
                    </MenubarItem>
                  </MenubarContent>
                </MenubarMenu>
              </Menubar>
              
              <Menubar className="border-none bg-transparent">
                <MenubarMenu>
                  <MenubarTrigger className="text-white hover:bg-white/20 border-none bg-transparent focus:bg-white/20 data-[state=open]:bg-white/20 transition-all duration-300">
                    <Settings className="w-4 h-4 mr-2 animate-spin-slow" />
                    Settings
                  </MenubarTrigger>
                  <MenubarContent>
                    <MenubarItem className="font-bold text-gray-800 cursor-pointer hover:bg-blue-50 transition-colors">
                      Profile
                    </MenubarItem>
                    <MenubarItem className="font-bold text-gray-800 cursor-pointer hover:bg-blue-50 transition-colors">
                      Account
                    </MenubarItem>
                    <MenubarItem className="font-bold text-gray-800 cursor-pointer hover:bg-blue-50 transition-colors">
                      Logout
                    </MenubarItem>
                  </MenubarContent>
                </MenubarMenu>
              </Menubar>
              
              <Button variant="ghost" className="text-white hover:bg-white/20 transition-all duration-300 hover:scale-105">
                <User className="w-4 h-4" />
              </Button>
            </div>
          </div>
        </div>
      </nav>

      {/* Main Content */}
      <div className="w-full py-8 relative z-10">
        <div className="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8">
          
          {/* Payment Section */}
          {showPaymentSection && (
            <div className="mb-8 space-y-6 animate-fade-in">
              <QuickPaymentOptions />
              <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
                <SubscriptionStatus />
                <PaymentHistory />
              </div>
            </div>
          )}

          <div className="animate-fade-in">
            <VoiceSearch onVoiceQuery={handleVoiceQuery} />
          </div>

          {/* Search Bar */}
          <div className="flex justify-center mb-6 animate-scale-in">
            <div className="w-full max-w-2xl relative">
              <input
                type="text"
                placeholder="Search for jobs..."
                value={searchQuery}
                onChange={(e) => setSearchQuery(e.target.value)}
                className="w-full px-4 py-3 pr-12 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent text-lg transition-all duration-300 hover:shadow-lg"
              />
              <Button
                onClick={handleSearch}
                className="absolute right-2 top-1/2 transform -translate-y-1/2 bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded transition-all duration-300 hover:scale-105"
              >
                <Search className="w-4 h-4" />
              </Button>
            </div>
          </div>

          {/* JOBS Title with animation */}
          <div className="text-center mb-6 animate-fade-in">
            <h2 className="text-3xl font-bold text-gray-800 animate-pulse">JOBS</h2>
            <div className="mt-2 flex justify-center">
              <div className="flex space-x-2">
                <div className="w-3 h-1 bg-blue-500 rounded animate-bounce"></div>
                <div className="w-3 h-1 bg-cyan-500 rounded animate-bounce animation-delay-200"></div>
                <div className="w-3 h-1 bg-purple-500 rounded animate-bounce animation-delay-400"></div>
              </div>
            </div>
          </div>

          <div className="animate-slide-in-right">
            <JobModeToggle 
              selectedModes={selectedJobModes} 
              onModeChange={handleJobModeChange} 
            />
          </div>

          <div className="animate-fade-in animation-delay-500">
            <AIResumeMatcher />
          </div>

          <div className="animate-scale-in animation-delay-700">
            <SkillGapAnalysis 
              userSkills={userSkills}
              jobs={allJobs}
              onSkillUpdate={handleSkillUpdate}
            />
          </div>

          {/* Filter Section */}
          <div className="flex justify-end mb-4 animate-fade-in">
            <Button 
              onClick={() => setShowFilters(!showFilters)}
              className="transition-all duration-300 hover:scale-105"
            >
              {showFilters ? 'Hide Filters' : 'Show Filters'}
            </Button>
          </div>

          {showFilters && (
            <div className="bg-white p-4 rounded shadow-md mb-6 animate-slide-in-right">
              <h3 className="text-xl font-semibold mb-4">Filter Jobs</h3>
              <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
                <div>
                  <label className="block text-gray-700 text-sm font-bold mb-2">State:</label>
                  <Select value={selectedState} onValueChange={setSelectedState}>
                    <SelectTrigger className="w-full transition-all duration-300 hover:shadow-md">
                      <SelectValue placeholder="Select a state" />
                    </SelectTrigger>
                    <SelectContent>
                      {states.map(state => (
                        <SelectItem key={state} value={state}>{state}</SelectItem>
                      ))}
                    </SelectContent>
                  </Select>
                </div>
                <div>
                  <label className="block text-gray-700 text-sm font-bold mb-2">Job Profile:</label>
                  <Select value={selectedJobProfile} onValueChange={setSelectedJobProfile}>
                    <SelectTrigger className="w-full transition-all duration-300 hover:shadow-md">
                      <SelectValue placeholder="Select a job profile" />
                    </SelectTrigger>
                    <SelectContent>
                      {jobProfiles.map(profile => (
                        <SelectItem key={profile} value={profile}>{profile}</SelectItem>
                      ))}
                    </SelectContent>
                  </Select>
                </div>
                <div>
                  <label className="block text-gray-700 text-sm font-bold mb-2">Salary Range:</label>
                  <div className="flex space-x-2">
                    <Input
                      type="number"
                      placeholder="From"
                      value={salaryFrom}
                      onChange={(e) => setSalaryFrom(e.target.value)}
                      className="w-1/2 transition-all duration-300 hover:shadow-md"
                    />
                    <Input
                      type="number"
                      placeholder="To"
                      value={salaryTo}
                      onChange={(e) => setSalaryTo(e.target.value)}
                      className="w-1/2 transition-all duration-300 hover:shadow-md"
                    />
                  </div>
                </div>
                <div>
                  <label className="block text-gray-700 text-sm font-bold mb-2">Category:</label>
                  <Select value={selectedCategory} onValueChange={setSelectedCategory}>
                    <SelectTrigger className="w-full transition-all duration-300 hover:shadow-md">
                      <SelectValue placeholder="Select a category" />
                    </SelectTrigger>
                    <SelectContent>
                      {allCategories.map(category => (
                        <SelectItem key={category} value={category}>{category}</SelectItem>
                      ))}
                    </SelectContent>
                  </Select>
                </div>
                <div>
                  <label className="block text-gray-700 text-sm font-bold mb-2">Date Posted:</label>
                  <Popover>
                    <PopoverTrigger asChild>
                      <Button
                        variant={"outline"}
                        className="w-full justify-start text-left font-normal transition-all duration-300 hover:shadow-md"
                      >
                        <CalendarIcon className="mr-2 h-4 w-4" />
                        {selectedDate ? (
                          new Date(selectedDate).toLocaleDateString()
                        ) : (
                          <span>Pick a date</span>
                        )}
                      </Button>
                    </PopoverTrigger>
                    <PopoverContent className="w-auto p-0" align="start">
                      <Calendar
                        mode="single"
                        selected={selectedDate}
                        onSelect={setSelectedDate}
                        disabled={(date) =>
                          date > new Date()
                        }
                        initialFocus
                      />
                    </PopoverContent>
                  </Popover>
                </div>
              </div>
              <Button 
                onClick={handleSearch} 
                className="mt-4 transition-all duration-300 hover:scale-105"
              >
                Apply Filters
              </Button>
            </div>
          )}

          {/* Job Results with animations */}
          <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
            {jobs.map((job, index) => (
              <Card 
                key={job.id} 
                className="bg-white shadow-md rounded-lg overflow-hidden hover-scale transition-all duration-300 hover:shadow-xl animate-fade-in"
                style={{animationDelay: `${index * 100}ms`}}
              >
                <CardContent className="p-6">
                  <div className="flex justify-between items-start mb-2">
                    <h3 className="text-xl font-semibold text-gray-800">{job.title}</h3>
                    <div className="w-2 h-2 bg-green-500 rounded-full animate-pulse"></div>
                  </div>
                  <p className="text-gray-600 mb-2">{job.company}</p>
                  <p className="text-gray-600 text-sm mb-3">{job.location}, {job.state}</p>
                  <div className="flex justify-between items-center">
                    <span className="text-gray-700">Salary: {job.salary}</span>
                    <Button className="transition-all duration-300 hover:scale-105">Apply Now</Button>
                  </div>
                </CardContent>
              </Card>
            ))}
          </div>

          <div className="animate-fade-in animation-delay-1000">
            <ChatBot />
          </div>
        </div>
      </div>
    </div>
  );
};


export default Index;
